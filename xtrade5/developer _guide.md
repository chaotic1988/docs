# Developer Guide

## 1 Overview



## 3 Events


### 3.2 Order Entry
|Field|Type|Description|
|---|---|---|
|account_id|Integer|Account ID|
|strategy_id|Integer|Strategy ID|
|order_id|Integer|Order ID|
|sid|Integer|Security ID|
|action|Integer|Buy/Sell|
|quantity|Integer|Quantity|
|price|Double|Limit price|

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
|action|Integer|Buy/Sell|
|quantity|Integer|Quantity|
|price|Double|Limit price|
|update_type|Integer|Order Update Type|



## 4 Enums

### 4.1 Event 
<!--stackedit_data:
eyJoaXN0b3J5IjpbNTIzNTYyOTQyLC0xMjU1OTcwNTkwXX0=
-->