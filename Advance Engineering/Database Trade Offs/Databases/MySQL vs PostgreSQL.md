# DataTypes
PostgreSQL is rich in terms of DataType than MySQL
# GeoLocation
PostgreSQL has extension named PostGIS for geosearch which is industry leader.
# Connection
PostgreSQL = one process per connections (it use pgBuncer)
MySQL = one thread per connections
# Page Size
PostgreSQL = 8KB
MySQL = 16KB
# Engine
MySQL has different storage engine innoDB, MySAM
PostgreSQL has only 1
# PostgreSQL suffers from vacuuming
```
PostgreSQL Vacuuming refers to a known trade-off in how PostgreSQL manages dead tuples—stale rows left behind after updates or deletes.

Why vacuuming is needed:

PostgreSQL uses MVCC (Multi-Version Concurrency Control), which keeps old versions of rows to support transactions and rollback. This means:

- An `UPDATE` creates a new row version, and marks the old one as dead.
    
- A `DELETE` marks the row as dead, but doesn't immediately remove it.
    

These dead tuples consume disk space and slow down queries. Hence, PostgreSQL needs VACUUM to clean them up.
```
# MySQL does multiple rebalancing than PostgreSQL
```
### 🛠️ MySQL (InnoDB):

- Uses clustered B+ trees for both tables and indexes.
- On heavy insert/update/delete:
    - Pages may split or merge frequently.
    - This leads to frequent rebalancing of the tree structure.
- Rebalancing keeps performance stable but adds I/O overhead.
    

---

### 🐘 PostgreSQL:

- Also uses B-trees for indexes, but:
    
    - Data storage is heap-based, not clustered.
        
    - Updates/deletes create dead tuples, and cleanup happens via vacuum, not rebalancing.
        
    - Less frequent index page splits unless under high write pressure.
```