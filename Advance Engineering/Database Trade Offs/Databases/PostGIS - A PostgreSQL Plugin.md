```
docker run --name my-postgis \
  -e POSTGRES_PASSWORD=password \
  -p 5432:5432 \
  -d postgres:15
```

```
CREATE TABLE bangalore_boundary (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    boundary GEOMETRY(POLYGON, 4326)  -- SRID 4326 for WGS84 lat/long
);
```

```
INSERT INTO bangalore_boundary (name, boundary) VALUES (
    'Bangalore',
    ST_GeomFromText(
        'POLYGON((77.4627 13.1455, 77.7523 13.1455, 77.7523 12.7471, 77.4627 12.7471, 77.4627 13.1455))',
        4326
    )
);
```

```
CREATE TABLE locations (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    coordinates GEOMETRY(POINT, 4326)
);
```

```
INSERT INTO locations (name, coordinates) VALUES
    ('Bangalore Palace', ST_SetSRID(ST_MakePoint(77.5925, 12.9977), 4326)),
    ('MG Road', ST_SetSRID(ST_MakePoint(77.6244, 12.9754), 4326)),
    ('Mumbai', ST_SetSRID(ST_MakePoint(72.8777, 19.0760), 4326)),
    ('Bangalore Airport', ST_SetSRID(ST_MakePoint(77.7069, 13.1986), 4326));
```

```
SELECT 
    l.name AS location,
    ST_AsText(l.coordinates) AS coordinates,
    CASE 
        WHEN ST_Within(l.coordinates, b.boundary) THEN 'Inside Bangalore'
        ELSE 'Outside Bangalore'
    END AS status
FROM 
    locations l,
    bangalore_boundary b
WHERE 
    b.name = 'Bangalore';
```