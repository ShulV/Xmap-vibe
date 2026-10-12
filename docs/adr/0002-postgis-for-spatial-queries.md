# PostGIS for Spatial Queries

We decided to use PostgreSQL with the PostGIS extension (`geometry(Point, 4326)`) and a spatial GiST index instead of simple separate latitude and longitude floating-point columns. This enables native, high-performance bounding-box viewport queries (`ST_MakeEnvelope`) and distance radius queries (`ST_DWithin`) directly in the database without ad-hoc mathematical calculations in the application layer.
