# Functional Requirements
1) User will able to see historic price of any item
2) User will able to set alert and price drops below limit
# Non Functional Requirements 
1) Highly Scalable
2) Highly Available
3) Eventual Consistent
4) Low Latency

# Assumptions
Number of product: 100 million
Number of users: 100 million
Number of seller: 100K

# API Design 
1) [GET] /api/product/:product_id/price-history?startTime={}&end={}&resolution={}
```
[
	{time:2024/07/03:00:00:00, avaragePrice: 10, high: 12, low: 9},
	{time:2024/07/03:00:00:00, avaragePrice: 10, high: 12, low: 9},
	{time:2024/07/03:00:00:00, avaragePrice: 10, high: 12, low: 9}
]
```
2) [POST] /api/price-drop/alert
```
{userID, productID, expectedPrice}

Response: OK {alertID}
```
3) [POST] /api/price-drop/alert/:alertID/pause
# Low Level Design
Alert 
PriceHistory 

# High Level Design

![[Amazon Price Tracker|1000]]