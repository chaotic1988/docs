# XTrade5 Developer Guide

## 1 Overview

### 1.1 Redis Stream
XTrade5 uses *Redis Stream* as communication middleware. Unlike common fire-and-forget queues, Redis streams persist messages, which enables multiple downstream readers to process the messages at the same time.

XTrade5 Clients and Hubs exchanges information through multiple Redis streams which carry different types of information like order requests and order responses. See Chapter 2 for details.

### 1.2 Security ID
Internally, XTrade5 uses intergers to identify securities for both market data feed and trading, instead of tickers like 000001. Tickers are mapped to a contiguous integer space starting from 0. To establish the mapping from tickers to internal ``sid``'s, securities are ordered by listed date, and are assigned a unique integer ``sid`` according the their relative positions. For example, 000005 is the first security listed in A-Share market, its ``sid`` is thus 0.

The advantages of using integer ``sid`` are:
1. Different market data providers and brokers may use different tickers for the same security. For example, some may add suffixes like .SH and .SZ to differentiate securities traded at different exchanges, while others don't. Using a unified integer security ID scheme can simplify programming. 
2. Because `sid`'s are in a contiguous integer range from 0, data structures like fix-sized arrays, instead of hash tables, can be employed to maintain sid-wise information, which  enables optimizations for performance.


### 1.3 Message
XTrade5 uses JSON for information exchange between Hubs and Clients. See Chapter 5 for details.

### 1.4 Order ID
The (``account_id``, ``strategy_id``, ``order_id``) tuple uniquely identifies an order. Usually, a client will be assigned a fixed ``account_id`` and ``strategy_id``, therefore it only needs to maintain a series of unique ``order_id``'s.


## 2 Streams

For clients, there are mainly two streams under concern: ``hub_in`` and ``hub_out``. Order requests(order entry or cancel) should be sent to ``hub_in``. On the other hand, clients needs to poll ``hub_out`` for order responses(order updates) received from Hubs. Note that clients of different strategy ids share the same ``hub_out``, therefore, a normal client needs to filter by ``account_id`` and ``strategy_id`` to get its own order responses.

## 3 Events


### 3.2 Order Entry
|Field|Type|Description|
|---|---|---|
|account_id|Integer|Account ID|
|strategy_id|Integer|Strategy ID|
|order_id|Integer|Order ID|
|sid|Integer|Security ID|
|action|Integer|Order action (see Section 4.2)|
|quantity|Integer|Quantity|
|price|Float|Limit price|

### 3.3 Order Cancel
|Field|Type|Description|
|---|---|---|
|account_id|Integer|Account ID|
|strategy_id|Integer|Strategy ID|
|order_id|Integer|Order ID|

### 3.4 Order Update
|Field|Type|Description|
|---|---|---|
|account_id|Integer|Account ID|
|strategy_id|Integer|Strategy ID|
|order_id|Integer|Order ID|
|sid|Integer|Security ID|
|action|Integer|Order Action (see Section 4.2)|
|quantity|Integer|Quantity|
|price|Float|Limit price|
|update_type|Integer|Order Update Type (see Section 4.3)|
|filled|Integer|Filled quantity|
|remaining|Integer|Remaining quantity|
|filled_value|Float|Filled value|
|commission|Float|Commission|
|error|Integer|Error code (see Section 4.4)|


## 4 Enums

### 4.1 Event Type
```c
enum EventType {
	ORDER_ENTRY  = 16,
	ORDER_UPDATE = 17,
	ORDER_CANCEL = 18
}; 
```

### 4.2 Order Action
```c
enum OrderAction {
	BUY = 0,
	SELL = 1
}; 
```

### 4.3 Order Update Type
```c
enum OrderUpdateType {
	ACCEPT = 0,
	REJECT = 1,
	FILL = 2,
	CANCEL = 3
}; 
```

### 4.4 Error Code
```c
enum OrderError {
	OK = 0,
	ERR_INVALID_SID = 1,
	ERR_INVALID_PRICE = 2,
	ERR_INVALID_QUANTITY = 3,
	ERR_INSUFFICIENT_HOLDINGS = 4,
	ERR_INSUFFICIENT_CASH = 5,
	ERR_UNKNOWN = 1000
}; 
```

## 5 Message Specification

The  message consists of two fields: the message type field `type` and the message payload `data`. The message type field is simply an EventType enum(See Section 4.1). The message payload can be one of the three event types: order entry, order cancel and order updates(See Chapter 3), and are serialized using JSON. For details of message format used in Redis Stream, check out [Introduction to Redis Stream](https://redis.io/topics/streams-intro).

Examples:
1. Order entry
```
- type
- 16
- data
- {"account_id": 0, "strategy_id": 0, "order_id": 5, "sid": 908, "action": 0, "quantity": 1000, "price": 11.43}
```
2. Order cancel
```
- type
- 18
- data
- {"account_id": 0, "strategy_id": 0, "order_id": 5}
```
3. Order update(order fill)
```
- type
- 17
- data
- {"account_id": 0, "strategy_id": 0, "order_id": 5, "sid": 908, "action": 0, "quantity": 1000, "price": 11.43, "update_type": 2, "filled": 300, "remaining": 700, "filled_value": 3429.0, "commission": 0, "error": 0}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU1ODY1NTkzOCwtNzk5NzY3ODcyXX0=
-->