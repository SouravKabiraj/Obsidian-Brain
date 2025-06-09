# Functional Requirement
1) Add Review with rating
2) Fetch Reviews of a food, resturent
3) Fetch Rating of a food, resturent
4) Evaluate Rating from all the reviews
5) Detect fake reviews
# Non Functional Requirements
1) Eventual Consistency (Monotonic Read)
2) High availability
3) Low Latency
4) Fault Tolerance
5) High Scalability
6) Security
# API Design
1) [POST] /api/review
```
RequestBody {userID,rating:0-10,review:"...",targetID:"...",targetType:"FOOD"}
Response OK {ID:"REV000001"}
```
2) [GET] /api/resturent/:resturentID/reviews?limit=10&sortBy=Newest
```
[{ID,userID,rating:8,review:"...",targetID:"...",targetType:"FOOD"},{...}]
```
# Low Level Design
Resturent
Food
Review
Rating

# High Level Design
![[Doordash Review|1000]]