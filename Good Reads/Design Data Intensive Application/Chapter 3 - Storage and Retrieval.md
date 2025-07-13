# Data Structures That Power Your Database
## 1. Fundamental Database Operations  
Databases must:  
- Store data durably when inserted or updated.  
- Retrieve data efficiently when queried.  

Key Insight:  
- Indexes accelerate reads but slow down writes (trade-off).  
- Storage engines are optimized for different workloads (OLTP vs. analytics).  

---

## 2. Simple Key-Value Storage & Indexing  

### (A) Append-Only Log  
- Write: Fast (append-only).  
- Read: Slow (O(n) scan).  
- Example:  
  ```bash
  db_set() { echo "$1,$2" >> database; }
  db_get() { grep "^$1," database | tail -n 1; }
  ```

### (B) Hash Index (Bitcask Model)  
- In-memory hash map maps keys to disk offsets.  
- Pros:  
  - Fast reads (O(1) lookup).  
  - High write throughput (sequential appends).  
- Cons:  
  - Entire key set must fit in RAM.  
  - No efficient range queries.  
- Optimizations:  
  - Segment files (break log into parts).  
  - Compaction (merge segments, discard duplicates).  
  - Crash recovery via snapshots.  

---
## 3. Log-Structured Storage: SSTables & LSM-Trees  

```
Examples: Cassendra, RiakDB
```

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#ffd8d8', 'edgeLabelBackground':'#fff'}}}%%

flowchart TD

subgraph Memory

A[Memtable In-memory Balanced Tree eg Red-Black Tree]

B[Write-Ahead Log WAL]

end

  

subgraph Disk

C[SSTable Segment 1 Sorted by Key]

D[SSTable Segment 2 Sorted by Key]

E[Older Segments]

F[Merged SSTable After Compaction]

end

  

subgraph Background Process

G[Compaction Merge + Sort + Remove Duplicates]

H[Bloom Filter]

end

  

%% Data Flow

WriteRequest -->|"1 Append to WAL"| B

WriteRequest -->|"2 Insert to Memtable"| A

A -->|"3 Memtable Full - Flush to Disk"| C

C -->|"4 Periodic Compaction"| G

D -->|"4 Periodic Compaction"| G

G -->|"5 Create Merged SSTable - Remove Duplicates"| F

  

%% Read Path

ReadRequest -->|"a Check Bloom Filter"| H

H -->|"Key May Exist"| A

A -->|"b Search Memtable"| ReadResponse

A -->|"Not Found"| C

C -->|"c Search Segments (Newest to Oldest)"| D

D -->|"Not Found"| E

E -->|"Found or Not Exist"| ReadResponse

  

%% Annotations

style G fill:#e3fdfd,stroke:#333

style H fill:#fef6e4,stroke:#333

  

%% Legend

legend[("

Key Processes:

1. Writes: WAL → Memtable → SSTable

2. Reads: Memtable → Newest SSTable → Older SSTables

3. Background: Compaction merges SSTables

")]
```

### Diagram Components Explained

1. Write Path (Red Arrows)
   - Incoming writes first go to the Write-Ahead Log (WAL) for crash recovery.
   - Then inserted into the Memtable (in-memory sorted structure).
   - When Memtable reaches size threshold, it's flushed to disk as an SSTable Segment.

2. Read Path (Blue Arrows)
   - Queries first check the Bloom Filter (quickly filters non-existent keys).
   - Search order: Memtable → Newest SSTable → Older SSTables (last write wins).
   - Sparse in-memory index enables binary search within SSTables.

3. Compaction Process (Green Box)
   - Merges multiple SSTables into one (like mergesort).
   - Key Benefits:
     - Removes duplicate keys (keeps only latest value).
     - Reduces disk space and improves read performance.
   - Runs in background without blocking writes.

1. Optimisations
   - Bloom Filter: Avoids unnecessary disk reads for non-existent keys.
   - Sorted SSTables: Enable efficient range scans and compression.
   - WAL: Ensures durability before writes are applied to Memtable.

### Key Advantages of LSM-Trees
- ✔ High Write Throughput: Sequential disk writes (no random updates).
- ✔ Efficient Compaction: Merging sorted files is CPU-friendly.
- ✔ Tunable Performance: Size-tiered vs. leveled compaction trade-offs.

---

## 4. B-Trees: The Industry Standard  
```
Examples: MySQL, PostgreSQL
```
### (A) Structure  
- Fixed-size pages (e.g., 4KB).  
- Balanced tree (fanout ~500 → depth of 3-4 for 256TB).  
- Operations:  
  - Insert/Split: May require rewriting multiple pages.  
  - Delete: Rebalance tree (complex but manageable).  
![[btree.png]]
### (B) Durability & Concurrency  
- Write-Ahead Log (WAL) ensures crash recovery.  
- Latches/locks for multi-threaded access.  

### (C) Optimizations  
- Copy-on-write (LMDB, avoids locks).  
- Shortened keys (reduce page size).  
- Clustered indexes (store data in leaf nodes).  

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#d8e8ff', 'edgeLabelBackground':'#fff'}}}%%

flowchart TD

subgraph DiskStructure

A[Root Page Persistent on Disk]

B[Branch Pages]

C[Leaf Pages Contain Actual Data]

D[Write-Ahead Log WAL]

end

  

subgraph Memory

E[Page Cache Cached B-Tree Nodes]

F[Buffer Pool]

end

  

subgraph Operations

G[Search]

H[Insert/Update]

I[Delete]

end

  

%% Structure Connections

A -->|Points to| B

B -->|Range Pointers| C

C -->|Linked List for Range Scans| C

  

%% Data Flow

H -->|"1 Log to WAL"| D

H -->|"2 Load Page to Buffer"| F

F -->|"3 Modify Page"| E

E -->|"4 Flush to Disk"| A

  

G -->|"a Binary Search from Root"| A

A -->|"b Traverse Branch Pages"| B

B -->|"c Find Leaf Page"| C

C -->|"d Return Data"| G

  

I -->|"Mark as Deleted + Rebalance"| C

C -->|"Page Underflow? Merge/Split"| B

B -->|"Propagate Changes Upwards"| A

  

%% Annotations

style D fill:#ffeaea,stroke:#333

style E fill:#e8ffd8,stroke:#333

  

legend[("

B-Tree Characteristics:

• Fixed-size pages (typically 4KB)

• Balanced tree (O(log n) access)

• WAL for crash recovery

• In-memory page cache

• Concurrency via latches

")]
```

---

## 5. LSM-Trees vs. B-Trees: A Detailed Comparison  

| Aspect              | LSM-Tree                               | B-Tree                        |     |
| ------------------- | -------------------------------------- | ----------------------------- | --- |
| Write Speed         | Faster (sequential writes)             | Slower (random writes + WAL)  |     |
| Read Speed          | Slower (check multiple SSTables)       | Faster (consistent structure) |     |
| Space Efficiency    | Better compression, less fragmentation | More fragmentation            |     |
| Write Amplification | High (compaction)                      | Moderate (page splits)        |     |
| Use Cases           | Write-heavy (logging, analytics)       | OLTP, strong consistency      |     |

Key Trade-offs:  
- LSM-Trees favor write throughput but suffer from compaction stalls.  
- B-Trees offer predictable read latency but slower writes.  

---

## 6. Advanced Indexing Techniques  

### (A) Secondary Indexes  
- Allow queries on non-primary keys (e.g., `user_id` index).  
- Can be implemented with B-Trees or LSM-Trees.  

### (B) Clustered vs. Covering Indexes  
- Clustered: Data stored directly in index (e.g., InnoDB primary key).  
- Covering: Index includes some columns (avoids heap lookup).  

### (C) Multi-Column & Spatial Indexes  
- Concatenated index (e.g., `(last_name, first_name)`).  
- R-trees for geospatial queries (PostGIS).

### (D) Full-Text Search (Fuzzy Indexes)  
- Edit distance (Lucene’s Levenshtein automaton).  
  ``Lucene engine (maintains Trie DS) is used in ElesticSearch
- Inverted indexes for document search.  

---

## 7. In-Memory Databases  
``Examples: Redis, MemSQL, VoltDB.  
- Pros:  
  - Avoid disk encoding overhead.  
  - Simpler data structures (e.g., Redis sets, queues).  
- Durability:  
  - Snapshots (periodic dumps).  
  - Log shipping (append-only log for recovery).  

---

## 8. Future Trends  
- Non-Volatile Memory (NVM): Could blur disk/memory distinction.  
- Hybrid designs: Anti-caching (evict cold data to disk).  

---

### Final Takeaways  
1. LSM-Trees excel in write-heavy, analytics workloads.  
2. B-Trees dominate OLTP, low-latency reads.  
3. Indexing strategy depends on access patterns (read vs. write ratio).  
4. Emerging storage tech (NVM, hybrid models) may reshape future DB designs.  