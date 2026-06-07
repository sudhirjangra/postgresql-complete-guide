# 05 — GiST Indexes (Generalized Search Tree)

> "GiST: the extensible framework for indexing anything that can't be sorted linearly."

---

## Table of Contents
1. [Learning Objectives](#learning-objectives)
2. [GiST Architecture](#gist-architecture)
3. [ASCII Diagram: GiST Tree Structure](#ascii-diagram-gist-tree-structure)
4. [Geometric Types with GiST](#geometric-types-with-gist)
5. [Range Types with GiST](#range-types-with-gist)
6. [Full-Text Search with GiST](#full-text-search-with-gist)
7. [Network Address Types](#network-address-types)
8. [GiST Operator Classes Reference](#gist-operator-classes-reference)
9. [GiST vs GIN Comparison](#gist-vs-gin-comparison)
10. [Creating GiST Indexes](#creating-gist-indexes)
11. [EXPLAIN Output for GiST Scans](#explain-output-for-gist-scans)
12. [PostGIS and GiST (Spatial Indexing)](#postgis-and-gist)
13. [Common Mistakes](#common-mistakes)
14. [Best Practices](#best-practices)
15. [Performance Considerations](#performance-considerations)
16. [Interview Questions & Answers](#interview-questions--answers)
17. [Exercises with Solutions](#exercises-with-solutions)
18. [Production Scenarios](#production-scenarios)
19. [Cross-References](#cross-references)

---

## Learning Objectives

After completing this section you will be able to:

- Explain GiST's generalized tree structure and key/value abstraction
- Describe how bounding boxes work in geometric GiST indexes
- Create GiST indexes for geometric types, range types, and full-text search
- Explain the difference between GiST and GIN and when to choose each
- Use GiST with PostGIS for spatial queries
- Identify GiST's key operators: containment, overlap, adjacency

---

## GiST Architecture

**GiST** (Generalized Search Tree) is a framework for building balanced tree indexes
on arbitrary data types. Unlike B-tree (which requires a total order) or GIN (which
requires decomposition into elements), GiST only requires that you define:

1. **Consistent**: Does this subtree possibly contain matching entries?
2. **Union**: What is the bounding value that covers all entries in this subtree?
3. **Penalty**: How much does adding this entry "expand" a page's key?
4. **PickSplit**: How to split a full page into two pages?

These four operations let GiST build a balanced tree for ANY type that can express
"does this region contain this value?" semantics.

### GiST Key Abstraction

In a B-tree, internal page keys are exact separator values. In GiST, internal page keys
are **bounding values** — they represent the union of all values beneath them.

For geometric data, bounding values are bounding boxes:
```
Internal page key: BOX(0,0,100,100)
Meaning: "All geometric objects in my subtree are within this box"
```

For range data, bounding values are merged ranges:
```
Internal page key: [1, 999]
Meaning: "All ranges in my subtree are sub-ranges of [1, 999]"
```

---

## ASCII Diagram: GiST Tree Structure

```
GiST index on locations(coords) where coords is POINT type

Heap rows:
  (0,1): POINT(10, 20)
  (0,2): POINT(15, 25)
  (0,3): POINT(80, 90)
  (0,4): POINT(50, 60)
  (0,5): POINT(55, 65)

GiST tree (bounding boxes shown):

                ROOT PAGE
        ┌─────────────────────────────────┐
        │  BOX(0,0,30,30) | BOX(40,50,90,100) │
        │      ptr1       |      ptr2          │
        └─────────────────────────────────────┘
               /                    \
              /                      \
  INTERNAL PAGE 1             INTERNAL PAGE 2
  ┌─────────────────────┐   ┌──────────────────────┐
  │ BOX(10,20,10,20)    │   │ BOX(50,60,55,65)     │
  │ BOX(15,25,15,25)    │   │ BOX(80,90,80,90)     │
  └─────────────────────┘   └──────────────────────┘
  (leaf level for points)
         |                          |
  ┌──────┴───────┐          ┌───────┴──────┐
  │(0,1) POINT   │          │(0,4) POINT   │
  │(10,20)       │          │(50,60)       │
  │(0,2) POINT   │          │(0,5) POINT   │
  │(15,25)       │          │(55,65)       │
  └──────────────┘          │(0,3) POINT   │
                            │(80,90)       │
                            └──────────────┘

Query: WHERE coords <@ BOX(0,0,20,30)  -- find points inside box
  1. Check root key BOX(0,0,30,30): intersects query box → follow ptr1
  2. Check root key BOX(40,50,90,100): does NOT intersect → PRUNE this subtree
  3. Check internal page 1 entries: BOX(10,20) ✓, BOX(15,25) ✓
  4. Return ctids (0,1) and (0,2)

GiST excels by PRUNING subtrees whose bounding box doesn't intersect the query
```

---

## Geometric Types with GiST

PostgreSQL has built-in geometric types that use GiST for indexing.

### Supported Geometric Types
- `POINT`: a single (x, y) point
- `LINE`, `LSEG`: infinite/finite lines
- `BOX`: axis-aligned rectangle
- `CIRCLE`: defined by center + radius
- `POLYGON`: arbitrary polygon
- `PATH`: open or closed sequence of points

```sql
-- Setup
CREATE TABLE locations (
    id SERIAL PRIMARY KEY,
    name TEXT,
    position POINT,
    area BOX,
    boundary POLYGON
);

-- GiST index
CREATE INDEX idx_locations_position ON locations USING GIST (position);
CREATE INDEX idx_locations_area ON locations USING GIST (area);

-- Geometric queries that use GiST:

-- Find all points within a box
SELECT * FROM locations
WHERE position <@ BOX '(0,0),(100,100)';

-- Find all boxes that overlap with a query box
SELECT * FROM locations
WHERE area && BOX '(10,10),(50,50)';

-- Find nearest points (requires <-> operator - closest point)
SELECT name, position <-> POINT '(50,50)' AS dist
FROM locations
ORDER BY dist
LIMIT 10;
-- Note: ORDER BY distance with LIMIT uses GiST index scan (KNN search)
```

### Geometric Operators for GiST
```sql
-- Point operators
@>   -- contains
<@   -- is contained by
&&   -- overlaps
~=   -- same as (point equality)
<->  -- distance (for KNN)

-- Box operators
&&   -- boxes overlap
@>   -- box contains box
<@   -- box is contained by box
~=   -- boxes are identical
```

---

## Range Types with GiST

Range types represent intervals and are natively indexed by GiST.

### Range Type Overview
```sql
-- Built-in range types
INT4RANGE, INT8RANGE  -- integer ranges
NUMRANGE             -- numeric ranges
TSRANGE, TSTZRANGE   -- timestamp ranges
DATERANGE            -- date ranges

-- Custom range type
CREATE TYPE float_range AS RANGE (subtype = float8);
```

### Range Type GiST Index
```sql
-- Booking/reservation table
CREATE TABLE reservations (
    id SERIAL PRIMARY KEY,
    room_id INTEGER,
    guest_name TEXT,
    stay DATERANGE NOT NULL,
    EXCLUDE USING GIST (room_id WITH =, stay WITH &&)  -- no double-booking!
);

CREATE INDEX idx_reservations_stay ON reservations USING GIST (stay);

-- Range queries that use GiST:

-- Find reservations overlapping August 2024
SELECT * FROM reservations
WHERE stay && DATERANGE('2024-08-01', '2024-08-31', '[]');

-- Find reservations contained within a range
SELECT * FROM reservations
WHERE stay <@ DATERANGE('2024-01-01', '2024-12-31');

-- Find reservations that start before a date
SELECT * FROM reservations
WHERE lower(stay) < '2024-06-01';
```

### EXCLUDE Constraint with GiST (Unique Ranges)
The exclusion constraint is one of GiST's killer features — it generalizes UNIQUE to
support overlap exclusion for range types:

```sql
-- Room booking: no two bookings for same room can overlap in time
ALTER TABLE reservations
ADD CONSTRAINT no_overlap
EXCLUDE USING GIST (room_id WITH =, stay WITH &&);

-- This prevents:
INSERT INTO reservations (room_id, guest_name, stay)
VALUES (1, 'Alice', '[2024-08-10, 2024-08-15)');
-- OK

INSERT INTO reservations (room_id, guest_name, stay)
VALUES (1, 'Bob', '[2024-08-12, 2024-08-17)');
-- ERROR: conflicting key value violates exclusion constraint
```

### Range Operators for GiST
```sql
@>    -- range contains element: '[1,10]'::int4range @> 5
<@    -- element contained in range: 5 <@ '[1,10]'::int4range
&&    -- ranges overlap: '[1,5]' && '[3,7]'
<<    -- strictly left of: '[1,3]' << '[5,7]'
>>    -- strictly right of: '[5,7]' >> '[1,3]'
&<    -- does not extend right of
&>    -- does not extend left of
-|-   -- ranges are adjacent: '[1,3]' -|- '[4,6]'
=     -- ranges are equal
```

---

## Full-Text Search with GiST

GiST can index `tsvector` using a signature-based approach.

```sql
-- GiST index for full-text search
CREATE INDEX idx_articles_fts_gist ON articles USING GIST (search_vector);

-- Same queries work as with GIN:
SELECT * FROM articles
WHERE search_vector @@ plainto_tsquery('english', 'postgresql performance');
```

### GiST vs GIN for Full-Text Search

| Aspect | GiST | GIN |
|--------|------|-----|
| Index build speed | Faster | Slower |
| Index size | Smaller | Larger |
| Query speed | Slower (false positives, recheck needed) | Faster |
| Update speed | Faster | Faster (with fastupdate) |
| False positives | Yes (lossy) | No (exact) |

GiST for full-text search uses **signature files** (bloom filter-like structures).
This means some index entries may be false positives that get filtered out during the
heap recheck step. This is why GiST is slower for full-text queries than GIN.

**Recommendation:** Use GIN for full-text search unless: (a) index build/update speed
is critical, (b) storage is very limited, (c) the data changes very frequently.

---

## Network Address Types

GiST supports network address types for subnet containment queries.

```sql
-- IP address containment
CREATE TABLE access_rules (
    id SERIAL PRIMARY KEY,
    network INET,
    action TEXT
);

CREATE INDEX idx_rules_network ON access_rules USING GIST (network inet_ops);

-- Find rules applying to a specific IP
SELECT * FROM access_rules
WHERE network >>= '192.168.1.100'::inet;  -- contains IP address

-- Find rules for a subnet
SELECT * FROM access_rules
WHERE network &&= '10.0.0.0/8'::inet;
```

---

## GiST Operator Classes Reference

| Data Type | Operator Class | Key Operators |
|-----------|---------------|---------------|
| `point`, `box`, `circle`, `polygon` | `point_ops`, `box_ops` | `&&`, `@>`, `<@`, `<->` |
| `int4range`, `daterange`, etc. | `range_ops` | `&&`, `@>`, `<@`, `<<`, `>>`, `-|-` |
| `tsvector` | `tsvector_ops` | `@@` |
| `inet`, `cidr` | `inet_ops` | `>>=`, `<<=`, `&&=` |
| `text` (via pg_trgm) | `gist_trgm_ops` | `%`, `~`, `LIKE`, `ILIKE` |

---

## GiST vs GIN Comparison

| Feature | GiST | GIN |
|---------|------|-----|
| Data model | "Overlap" / bounding values | "Contains" / posting lists |
| Geometry support | Yes (native) | No |
| Range type support | Yes (native) | Limited |
| JSONB support | No | Yes |
| Array support | No | Yes |
| Full-text search | Yes (lossy) | Yes (exact) |
| Index size | Smaller | Larger |
| Build speed | Faster | Slower |
| Query speed | Varies (may have false positives) | Generally faster |
| Exclusion constraints | Yes | No |
| KNN / nearest-neighbor | Yes | No |

**Decision rule:** Use GiST for geometric, spatial, network, or range types. Use GIN for
multi-valued types (arrays, JSONB) and full-text search when query speed matters most.

---

## Creating GiST Indexes

```sql
-- Geometric
CREATE INDEX CONCURRENTLY idx_stores_location ON stores USING GIST (location);

-- Range type
CREATE INDEX CONCURRENTLY idx_bookings_period ON bookings USING GIST (period);

-- Full-text (prefer GIN, but GiST is lighter on writes)
CREATE INDEX CONCURRENTLY idx_docs_vector ON documents USING GIST (search_vector);

-- Network addresses
CREATE INDEX CONCURRENTLY idx_firewall_network ON firewall_rules
USING GIST (network inet_ops);

-- pg_trgm with GiST (smaller than GIN trgm, slightly slower)
CREATE EXTENSION IF NOT EXISTS pg_trgm;
CREATE INDEX CONCURRENTLY idx_products_name_gist ON products
USING GIST (name gist_trgm_ops);

-- With fill factor
CREATE INDEX idx_events_bbox ON events USING GIST (bounding_box)
WITH (fillfactor = 80);
```

---

## EXPLAIN Output for GiST Scans

```sql
-- Reservation overlap check
EXPLAIN (ANALYZE, BUFFERS)
SELECT * FROM reservations
WHERE stay && DATERANGE('2024-08-01', '2024-08-31', '[]');
```

```
Bitmap Heap Scan on reservations  (cost=4.56..89.23 rows=15 width=64)
                                   (actual time=0.123..0.456 rows=14 loops=1)
  Recheck Cond: (stay && '[2024-08-01,2024-09-01)'::daterange)
  Heap Blocks: exact=12
  Buffers: shared hit=18
  ->  Bitmap Index Scan on idx_reservations_stay
          (cost=0.00..4.56 rows=15 width=0)
          (actual time=0.089..0.089 rows=14 loops=1)
        Index Cond: (stay && '[2024-08-01,2024-09-01)'::daterange)
Planning Time: 0.234 ms
Execution Time: 0.512 ms
```

### KNN (K-Nearest-Neighbor) Scan
```sql
EXPLAIN (ANALYZE, BUFFERS)
SELECT name, position <-> POINT'(50,50)' AS dist
FROM locations ORDER BY dist LIMIT 5;
```

```
Limit  (cost=0.28..1.34 rows=5 width=40)
       (actual time=0.123..0.189 rows=5 loops=1)
  ->  Index Scan using idx_locations_position on locations
          (cost=0.28..10234.56 rows=50000 width=40)
          (actual time=0.121..0.183 rows=5 loops=1)
        Order By: (position <-> '(50,50)'::point)
```

Note: KNN scans use `Index Scan` (not Bitmap), ordered by distance. The index is
traversed in distance order, stopping after 5 results — very efficient.

---

## PostGIS and GiST

PostGIS extends PostgreSQL with full geospatial support using GiST for spatial indexing.

```sql
-- Install PostGIS
CREATE EXTENSION IF NOT EXISTS postgis;

-- Table with geometry column
CREATE TABLE restaurants (
    id SERIAL PRIMARY KEY,
    name TEXT,
    geom GEOMETRY(POINT, 4326)  -- WGS84 lat/lon points
);

-- GiST index (PostGIS uses a 2D bounding box index)
CREATE INDEX idx_restaurants_geom ON restaurants USING GIST (geom);

-- Spatial queries:

-- Find restaurants within 5km of a point
SELECT name, ST_Distance(geom::geography, ST_MakePoint(-73.9857, 40.7484)::geography) AS dist_m
FROM restaurants
WHERE ST_DWithin(geom::geography, ST_MakePoint(-73.9857, 40.7484)::geography, 5000)
ORDER BY dist_m
LIMIT 10;

-- Find restaurants within a polygon (neighborhood boundary)
SELECT * FROM restaurants r
JOIN neighborhoods n ON ST_Within(r.geom, n.boundary)
WHERE n.name = 'Manhattan';

-- BOUNDING BOX operators (faster, less precise, use for pre-filtering):
-- && operator: bounding boxes overlap
SELECT * FROM restaurants
WHERE geom && ST_MakeEnvelope(-74.02, 40.70, -73.95, 40.78, 4326);
```

---

## Common Mistakes

### 1. Using GiST for JSONB or arrays (GIN is better)
```sql
-- WRONG: GiST doesn't have native JSONB support
-- Use GIN for JSONB
CREATE INDEX USING GIN (data);  -- correct
```

### 2. Forgetting that GiST full-text search has false positives
GiST uses a lossy compression for tsvector. The "Recheck Cond" in EXPLAIN shows that
heap rows must be re-checked. For large tables with many false positives, GiST can be
significantly slower than GIN for full-text queries.

### 3. Not using exclusion constraints for range overlap prevention
Many developers write complex application logic to prevent overlapping bookings.
PostgreSQL's `EXCLUDE USING GIST` handles this atomically at the database level.

### 4. Using B-tree for range type columns
```sql
-- WRONG: B-tree doesn't support range operators
CREATE INDEX ON reservations(stay);  -- B-tree, useless for && queries

-- CORRECT:
CREATE INDEX ON reservations USING GIST (stay);
```

### 5. Not installing pg_trgm before creating trigram GiST indexes
```sql
-- Will fail without the extension
CREATE INDEX ON products USING GIST (name gist_trgm_ops);
-- ERROR: operator class "gist_trgm_ops" does not exist
-- Fix: CREATE EXTENSION pg_trgm;
```

---

## Best Practices

1. **Always use GiST for geometric and spatial data** — it's the only native option
   for point, box, polygon, circle types.

2. **Always use GiST for range types** — especially with exclusion constraints for
   booking/reservation systems.

3. **Prefer GIN over GiST for full-text search** unless write performance is critical.

4. **Use PostGIS for serious geospatial work** — it adds optimized GiST indexes with
   many geometry functions that the built-in types lack.

5. **KNN queries with LIMIT are very efficient with GiST** — the planner uses an
   incremental distance traversal that stops early.

6. **For spatial data: use ST_DWithin instead of ST_Distance < X** — `ST_DWithin` is
   index-aware; `ST_Distance` computed on all rows is not.

---

## Performance Considerations

### GiST Build Parameters
```sql
SET maintenance_work_mem = '1GB';
-- GiST builds are CPU-intensive; more memory reduces the number of passes
```

### Index Size
GiST indexes are generally smaller than GIN because internal pages store bounding
values (compact summaries) rather than full entry lists.

### Bounding Box Quality
GiST performance degrades when bounding boxes at internal nodes become too large
(poor discrimination). For point data distributed uniformly, performance is good.
For clustered data (e.g., all points in one city), all boxes nearly coincide and
the tree provides poor pruning — more rows are examined than necessary.

```sql
-- Check GiST effectiveness by examining actual vs estimated rows
EXPLAIN (ANALYZE, BUFFERS)
SELECT * FROM locations WHERE position <@ BOX '(0,0),(10,10)';
-- If actual rows >> estimated rows, statistics may need ANALYZE
```

---

## Interview Questions & Answers

**Q1. What is the key difference between GiST and B-tree in how they represent internal page keys?**

A: B-tree internal pages store exact separator keys — values that are in the data. GiST
internal pages store bounding values — compact summaries (like bounding boxes) that
represent the union of all values in the subtree below. For geometric data, this is a
bounding rectangle. For range types, it's the merged range. This means GiST can express
"this subtree contains things in this spatial region" for pruning, without requiring a
total ordering of the data type.

---

**Q2. What is a GiST exclusion constraint and what problem does it solve?**

A: An exclusion constraint using GiST ensures that no two rows in the table satisfy a
specified overlap condition. The most common use case is preventing double-booking in
scheduling systems: `EXCLUDE USING GIST (resource_id WITH =, time_period WITH &&)` ensures
that no two rows with the same resource_id have overlapping time_period ranges. This is
more expressive than UNIQUE (which only handles equality) and is enforced atomically by
the database without requiring application-level lock management.

---

**Q3. When would you use GiST over GIN for full-text search?**

A: GiST is preferred over GIN for full-text search when: (1) The table has very frequent
inserts or updates — GiST updates faster than GIN because each row has a single compact
index entry, while GIN must update potentially many posting lists. (2) Storage space is
very limited — GiST tsvector indexes are smaller than GIN. (3) The query load is modest
and the extra recheck cost of GiST's lossy compression is acceptable. GIN is preferred
when query speed is the priority.

---

**Q4. What is KNN search and how does GiST support it?**

A: KNN (K-Nearest-Neighbor) search finds the K closest data points to a query point.
GiST supports this through an incremental distance traversal: the planner uses the
`<->` (distance) operator to order results, and the index is traversed in order of
increasing bounding box distance, pruning subtrees that cannot contain a closer point
than already found. Combined with LIMIT, this means only a small fraction of the index
is accessed. B-tree cannot do this; it has no concept of distance or spatial proximity.

---

**Q5. How does the Recheck step work for GiST and why is it needed?**

A: GiST uses bounding values (like bounding boxes) at internal pages. When a query tests
whether a bounding box overlaps the query region, the test is conservative: if the
bounding box overlaps, the subtree might contain matches. This can produce false positives
— index entries that pass the bounding box test but don't actually match the precise
condition. The "Recheck Cond" step re-evaluates the exact predicate on the heap rows
returned by the GiST scan to filter out these false positives. For GIN (which is exact),
no recheck is needed.

---

## Exercises with Solutions

### Exercise 1
Design a room booking system that prevents double-booking using GiST. Include the table schema, index, and constraint.

**Solution:**
```sql
CREATE TABLE room_bookings (
    id SERIAL PRIMARY KEY,
    room_id INTEGER NOT NULL,
    guest_name TEXT NOT NULL,
    booking_period TSTZRANGE NOT NULL,
    status TEXT DEFAULT 'confirmed'
);

-- Exclusion constraint: same room, overlapping time = not allowed
ALTER TABLE room_bookings
ADD CONSTRAINT no_double_booking
EXCLUDE USING GIST (room_id WITH =, booking_period WITH &&)
WHERE (status != 'cancelled');

-- Regular GiST index for queries
CREATE INDEX idx_bookings_period ON room_bookings USING GIST (booking_period);
CREATE INDEX idx_bookings_room ON room_bookings (room_id);

-- Test:
INSERT INTO room_bookings (room_id, guest_name, booking_period)
VALUES (101, 'Alice', '[2024-08-10 14:00, 2024-08-12 11:00)');

INSERT INTO room_bookings (room_id, guest_name, booking_period)
VALUES (101, 'Bob', '[2024-08-11 14:00, 2024-08-13 11:00)');
-- ERROR: conflicting key value violates exclusion constraint "no_double_booking"
```

---

## Production Scenarios

### Scenario 1: Geospatial Store Locator
A retail chain with 50,000 stores worldwide needs a "find nearest stores" API endpoint.

```sql
CREATE EXTENSION IF NOT EXISTS postgis;

CREATE TABLE stores (
    id SERIAL PRIMARY KEY,
    name TEXT,
    address TEXT,
    geom GEOMETRY(POINT, 4326)
);

CREATE INDEX idx_stores_geom ON stores USING GIST (geom);

-- API query: 10 nearest stores to user location
SELECT id, name, address,
       ST_Distance(geom::geography, ST_MakePoint($lon, $lat)::geography) AS distance_m
FROM stores
WHERE ST_DWithin(geom::geography, ST_MakePoint($lon, $lat)::geography, 50000)  -- within 50km
ORDER BY geom <-> ST_MakePoint($lon, $lat)::geometry  -- KNN ordering
LIMIT 10;
-- Result: <1ms with GiST index on 50K rows
```

---

## Cross-References

- [04_gin_indexes.md](04_gin_indexes.md) — GIN for arrays, JSONB, full-text
- [01_index_fundamentals.md](01_index_fundamentals.md) — Index types overview
- [../08_Query_Optimization/03_scan_types.md](../08_Query_Optimization/03_scan_types.md) — Scan types
