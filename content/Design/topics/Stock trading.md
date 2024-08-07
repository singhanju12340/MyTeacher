**Requirements:**
1. Buy and sell financial products via exchange and see their existing positions
2. user should be able to see real time value of their asset and total asset
3. minimise number of server listning to the exchange

**NFR:**
	single area 
	high scalable
	high available
	low latency
	

**Capacity Estimates:**
1. 100M total user
2. 10 %  of 100M i.e 10M daily active users for trade
3. 1 user does 5 trade a day
4. 50M trade per day(6 hours a day)= 2K write per sec 
5. 30% of 100M i.e (30M * 6*3600) = 6K read
6. 100 different instruments(stock/options ) in portfolio
7. maintain single active connection per client

maintain user connection list to fetch user session connection.

**API design**
stockTicker - NSE/NYCE
	placeOrder(customerId, stockTicker/exchangeCode, type, quantity, placedAt)
	fetchStockDetail(String stockId)
	fetchCustomerPortfolio(customerId)
	setTrigger(customerId, exchangeCode, stockTicker, quantity, threshold, condition)


**DB designs:**

Customer: id, name

``` java
Stock: 
	id, 
	exchange-BSE/NSE,
	 ticker,
	 price
```


``` java
CustomerStockOrders-
	orderId, 
	customerId, 
	stockId,
	 currentPrice,
	  buy/sell, 
	  status(PLACED/FAILED/STATUS),
	quantity, 
	placedAt
	price

Triggers:
	customerId,
	stockId,
	time,
	quantity,
	condition,
	threshold,
	price
	
CustomerPortfolio:
	id
	customerId,
	List<CustomerStock>
	 
	


StockPriceHistory{
	stockId,
	price,
	timeInmiliSec
}
```
``


[[stockExchange]]
