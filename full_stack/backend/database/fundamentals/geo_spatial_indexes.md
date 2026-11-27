<div align='center'>
    <h1> Indexing & Query Optimization </h1>
    <h2> Geospatial Indexes </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
  - [Key Features of Geospatial Indexes](#key-features-of-geospatial-indexes)
  - [Applications of Geospatial Indexes](#applications-of-geospatial-indexes)
- [Geohash](#geohash)
- [Quadtree](#quadtree)
- [Applications of Geohashes and Quadtrees](#applications-of-geohashes-and-quadtrees)
- [References](#references)

# Introduction

Geospatial indexes are specialized data structures designed to efficiently store, query, and retrieve spatial data, such as geographical coordinates (longitude and latitude).

Redis, PostGIS, Elasticsearch, and MongoDB all natively support geospatial indexing.

---

## Key Features of Geospatial Indexes

- Efficient Query Performance: geospatial indexes allow for rapid querying of spatial data by reducing the search space. For example, a query to find all points within a 10-mile radius of a location can be executed faster with a geospatial index compared to a `full table scan`.

- They are typically more efficient than creating separate indexes for latitude and longitude, as geospatial indexes preserve the two-dimensional relationships of the data, allowing for `spatial queries`.

---

## Applications of Geospatial Indexes

Applications include:

- Storing locations on a map.
- Searching for places within a specific geographical area.
- Calculating distances between spatial coordinates.

---

# Geohash

A geohash is a geospatial index that converts geographic coordinates (latitude and longitude) into a short alphanumeric string. Geohashes use a base-32 encoding system, i.e., each character in the geohash string represents a subdivision of the previous cell into 32 smaller cells. 

Each character in a geohash represents a specific region or cell on Earth's surface, with subsequent characters narrowing the region (more characters mean a smaller cell and a more precise location). Geohashing allows for fast geospatial searches, such as finding nearby points of interest or performing proximity queries, by comparing string prefixes.

---

# Quadtree

A QuadTree is a [tree data structure](https://github.com/camponogaraviera/ds-and-algo/blob/main/ds_and_algo/theory/data_structures/trees/trees.md) where each node has four children. It is used to recursively partition a two-dimensional space into four equal-sized quadrants or regions, allowing efficient storage, querying, and retrieval of spatial data such as points, lines, curves, and areas.

Why QuadTrees?

Quadtrees are `less CPU-intensive to compute than raw latitude and longitude` data when stored without any spatial indexing structure, since in that case it would require a linear search to locate a specific point or to compute distances.

Quadtrees are particularly useful for range queries, nearest neighbor searches, and collision detection in applications such as geographic information systems (GIS), computer graphics, and gaming.

They can be classified into:

- Region quadtree.
- Point quadtree.
- Point-region (PR) quadtree.
- Edge quadtree.
- Polygonal map (PM) quadtree.
- Compressed quadtree.

Note: ["A range query is a common database operation that retrieves all records where some value is between an upper and lower boundary."](<https://en.wikipedia.org/wiki/Range_query_(database)>)

---

# Applications of Geohashes and Quadtrees

- Geohashes:
  - Used in location-based services, such as finding nearby drivers (e.g., Uber), restaurants, gas stations, hotels, and ATMs by encoding raw latitude and longitude into an alphanumeric string.

- QuadTrees:
  - Used for spatial partitioning, terrain representation, and multi-resolution image storage in applications such as GIS, mapping, and computer graphics.

Note: Both Geohash and QuadTree support efficient route calculations (e.g., road networks), but graph-based data structures are more common for pathfinding using [Dijkstra's](https://github.com/camponogaraviera/ds-and-algo/blob/main/ds_and_algo/theory/algorithms/shortest_path/dijkstra.md) or [A\*](https://github.com/camponogaraviera/ds-and-algo/blob/main/ds_and_algo/theory/algorithms/shortest_path/a_star.md) algorithms.

---

# References

[1] https://www.mongodb.com/docs/manual/core/indexes/index-types/index-geospatial/

[2] https://handwiki.org/wiki/Earth:Geohash

[3] https://handwiki.org/wiki/Quadtree?utm_source=chatgpt.com
