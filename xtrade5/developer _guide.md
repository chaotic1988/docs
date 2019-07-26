# Developer Guide

## 1 Overview

### 1.1 Redis Stream
XTrade5 uses Redis Stream as communication middleware. *Clients* and *Hubs* exchanges information through multiple Redis streams which carry different types of information like order requests and order responses. See Chapter 2 for details.

### 1.2 Security ID

### 1.3


## 2 Streams

For clients, there are mainly two streams to concern: ``hub_in`` and ``hub_out``. Order requests(entry or cancel) should be sent to ``hub_in``. On the other lients needs to poll ``hub_out``

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
enum OrderAction {
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
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIxMjAxODIxLC04ODc4MDMwMzUsMjE3Mj
cwMTkxLC0xMjU1OTcwNTkwXX0=
-->