**Design a food delivery search system (e.g., Uber Eats) that:**

1. Handles **geospatial queries** (e.g., "burritos near me" with dynamic delivery boundaries).
2. Prioritizes **high recall** (fetching all relevant stores) and **high precision** (ranking top results accurately).
3. Supports **personalization** (user history, dietary preferences) and **real-time updates** (menu/pricing changes).
---
#### **1. High-Level Architecture**

![[diagram1|150]]

**Explanation**:
1. **Retrieval Layer**: Fetches candidate stores using geospatial and text filters.
2. **First-Pass Ranker**: Applies lightweight ranking (text relevance, delivery time).
3. **Hydration Layer**: Adds real-time data (e.g., menu changes, store images).
4. **Second-Pass Ranker**: Uses ML models for personalisation and business rules.
---
#### **2. Detailed Workflows**
##### **a. Retrieval Layer Flow**
![[diagram2]]

**Steps Explained**:
1. **Geosharding**: The query’s location ("SF") is mapped to H3 hexagons to identify relevant shards.
2. **Parallel Index Query**: Each shard (e.g., downtown SF) queries its Lucene index for matching stores.
3. **Combiner**: Aggregates results and returns the top 200 candidates to balance recall and latency.

##### **b. First-Pass Ranking Flow**

![[diagram3|1000]]
**Steps Explained**:

1. **Filtering**: Removes closed stores or those with long delivery times.
2. **Scoring**: Uses TF-IDF to rank stores by text relevance (e.g., "sushi" in menu).
3. **Top Candidates**: Passes the top 100 stores to the hydration layer.

##### **c. Hydration Layer Flow**
![[diagram4|1000]]

**Steps Explained**:
1. **Real-Time Data Fetch**: Enriches candidates with:
    - **Dynamic Data**: Menu changes from a key-value store (e.g., Redis).
    - **User Context**: Dietary preferences (e.g., vegan) from user profiles.
    - **Media**: Images/ratings cached in a CDN.
##### **d. Second-Pass Ranking Flow**

![[diagram5]]

**Steps Explained**:

1. **ML Model**: Scores stores based on user history (e.g., prefers sushi restaurants with high ratings).
2. **Business Rules**: Boosts stores with promotions or Uber One membership benefits.
3. **Final Ranking**: Combines scores to return the top 10 results.

---
#### **3. Scaling Challenges & Solutions**
- **Geospatial Complexity**:
    - **Problem**: Cross-city deliveries (e.g., SF → Berkeley).
    - **Solution**: Query adjacent H3 hexagons and deduplicate results.
- **Latency Spikes**:
    - **Problem**: Fetching 200+ candidates increased P50 latency by 4x.
    - **Solution**: Optimize Lucene’s `IndexSearcher` and benchmark shard sizes.
---
#### **4. Metrics & Validation**

| Metric        | Target | How to Measure                               |
| ------------- | ------ | -------------------------------------------- |
| Recall        | >95%   | % of relevant stores in retrieved candidates |
| Precision     | >80%   | % of top-10 results users click/order        |
| Latency (P99) | <100ms | Distributed tracing (e.g., Jaeger)           |

---
#### **5. Key Tradeoffs**
1. **Recall vs. Latency**: Fetching more candidates improves recall but hurts latency.    
    - _Uber’s Fix_: Early termination after 200 candidates.
2. **Freshness vs. Cost**: Real-time hydration requires costly infrastructure.
    - _Uber’s Fix_: Use Redis for low-latency updates and CDN for static assets.
---
### **Why This Answer Works**
- **End-to-End Workflows**: Diagrams show how data flows from query to results.
- **Uber-Specific Tactics**: Geosharding (H3), two-phase ranking, and hydration.
- **Metrics-Driven**: Defines how to validate recall, precision, and latency.

This mirrors how Uber Eats scaled its search system to 1M+ merchants while keeping latency under 100ms. Let me know if you’d to dive deeper into any component!


# Reference
1) [H3 by Uber](https://www.uber.com/en-IN/blog/h3/)