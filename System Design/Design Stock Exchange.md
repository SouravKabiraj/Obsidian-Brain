# Functional Requirements 
1) User will able to buy a stock (with lowest selling price) + custom unit
2) User will able to sell a stock (with custom price) + custom unit
3) User will able to set auto buy at selected price
# Non Functional Requirements
1) Highly Scalable ++
2) Consistency has higher priority +++
3) Availability at-least 99.99% ++
4) Security +++
5) Falut Tolerant
6) Low Latency

# API Design 
[POST] /api/order
``{user_id, symbol, type=BUY,SELL, price, qty, expairyTime}
[GET] /api/stock/:symbol
``{symbol, price}
# Assumptions
Number of stocks = 10K
Number of DAU = 1 million / day
Number of Order = 1 million / day

# Components
Stock
User
Order (Buy/Sell)
Payment

# HLD
![[stock_exchange|1000]]