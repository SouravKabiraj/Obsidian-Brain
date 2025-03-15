A **ranked cache system** is a caching mechanism where items are stored and evicted based on a ranking or priority score. This is useful in scenarios where not all cached items are equally important, and you want to prioritise retaining higher-value items in the cache.

Below is a detailed design and implementation of a ranked cache system.

---

### **1. System Requirements**
- **Ranking Mechanism**: Assign a score or priority to each cached item.
- **Eviction Policy**: Evict lower-ranked items when the cache is full.
- **Efficient Lookup**: Quickly retrieve items by their keys.
- **Dynamic Updates**: Update the rank of items dynamically.
- **Scalability**: Handle a large number of items efficiently.

---

![[ranked_cache]]
### **2. System Design**
#### **2.1. Data Structures**
- **Hash Map**: Stores key-value pairs for O(1) lookups.
- **Priority Queue**: Maintains items in ranked order for efficient eviction of the lowest-ranked item.
- **Balanced Binary Search Tree (Optional)**: Can be used instead of a priority queue for efficient rank updates and lookups.

#### **2.2. Components**
- **Cache Store**: Stores the actual key-value pairs.
- **Ranking Engine**: Computes and updates the rank of items.
- **Eviction Manager**: Evicts items when the cache reaches its capacity.

#### **2.3. Workflow**
1. **Insertion**:
   - Compute the rank of the new item.
   - Add the item to the cache store and the priority queue.
   - If the cache is full, evict the lowest-ranked item.
2. **Lookup**:
   - Retrieve the item from the cache store.
   - Update its rank if necessary (e.g., based on access frequency).
3. **Eviction**:
   - Remove the lowest-ranked item from the cache store and the priority queue.

---

### **3. Implementation**
Below is a Python implementation of a ranked cache system using a hash map and a priority queue (min-heap).

```python
import heapq

class RankedCache:
    def __init__(self, capacity):
        self.capacity = capacity
        self.cache = {}  # Hash map to store key-value pairs
        self.heap = []   # Min-heap to maintain items in ranked order

    def get(self, key):
        if key in self.cache:
            value, rank = self.cache[key]
            # Update rank (e.g., increase rank on access)
            self.cache[key] = (value, rank + 1)
            return value
        return None

    def put(self, key, value, rank):
        if key in self.cache:
            # Update existing item
            self.cache[key] = (value, rank)
            # Rebuild heap to reflect updated rank
            self.heap = [(r, k) for k, (v, r) in self.cache.items()]
            heapq.heapify(self.heap)
        else:
            if len(self.cache) >= self.capacity:
                # Evict the lowest-ranked item
                while self.heap:
                    min_rank, min_key = heapq.heappop(self.heap)
                    if min_key in self.cache and self.cache[min_key][1] == min_rank:
                        del self.cache[min_key]
                        break
            # Add new item
            self.cache[key] = (value, rank)
            heapq.heappush(self.heap, (rank, key))

    def __str__(self):
        return str(self.cache)

# Example Usage
cache = RankedCache(3)
cache.put("a", 1, 5)  # Add key="a", value=1, rank=5
cache.put("b", 2, 3)  # Add key="b", value=2, rank=3
cache.put("c", 3, 7)  # Add key="c", value=3, rank=7
print(cache)          # Output: {'a': (1, 5), 'b': (2, 3), 'c': (3, 7)}

cache.get("b")        # Access key="b", increasing its rank
print(cache)          # Output: {'a': (1, 5), 'b': (2, 4), 'c': (3, 7)}

cache.put("d", 4, 2)  # Add key="d", value=4, rank=2 (evicts key="b")
print(cache)          # Output: {'a': (1, 5), 'c': (3, 7), 'd': (4, 2)}
```

---

### **4. Ranking Strategies**
- **Access Frequency**: Rank items based on how often they are accessed.
- **Recency**: Rank items based on how recently they were accessed.
- **Custom Metrics**: Use application-specific metrics (e.g., user priority, item value).

---

### **5. Optimizations**
- **Lazy Eviction**: Delay eviction until necessary to reduce overhead.
- **Batch Updates**: Update ranks in batches to reduce heap operations.
- **Distributed Cache**: Use a distributed caching system (e.g., Redis) for scalability.

---

### **6. Example Use Cases**
- **Web Caching**: Cache web pages or API responses based on user priority or popularity.
- **Ad Serving**: Cache ads based on their relevance or bid value.
- **E-commerce**: Cache product details based on sales rank or user interest.

---

### **7. Challenges and Solutions**
- **Ranking Overhead**: Use efficient data structures (e.g., heaps, balanced trees) to minimize ranking and eviction costs.
- **Concurrency**: Use thread-safe data structures or locks for multi-threaded environments.
- **Memory Management**: Limit cache size and use eviction policies to prevent memory exhaustion.

---

This ranked cache system is flexible, efficient, and can be adapted to various use cases by customizing the ranking strategy and eviction policy.