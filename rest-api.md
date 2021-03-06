# Public Rest API for Piexgo 

# General API Information
* The  base prod-endpoint is: **https://api.piexgo.com**
* The base test-endpoint is: **https://api.piexgo.co**
* All endpoints return either a JSON object or array.


* Any endpoint can return an ERROR; the error payload is as follows:
```javascript
{
  "code": 1017,
  "msg": "Invalid symbol."
}
```


# Endpoint security type
* Each endpoint has a security type that determines the how you will
  interact with it.
* API-keys are passed into the Rest API via the `KEY` and `signature`
  header.
* API-keys and secret-keys **are case sensitive**.
* By default, API-keys can access all secure routes.




* `TRADE` and `USER_DATA` endpoints are `SIGNED` endpoints.

# SIGNED Endpoint security
* `SIGNED` endpoints require an additional parameter, `signature`, to be
  sent in the  `query string`.
* Endpoints use `HMAC SHA256` signatures. The `HMAC SHA256 signature` is a keyed `HMAC SHA256` operation.
  Use your `secretKey` as the key and `totalParams` as the value for the HMAC operation.
* The `signature` is **case sensitive**.


## Timing security
* TODO 



## SIGNED Endpoint Examples for POST /api/v1/order
Here is a step-by-step example of how to send a vaild signed payload.

Key | Value
------------ | ------------
apiKey | 23db31c6-6fd7-4041-9388-81dea6a05086
secretKey | 7bea08b911022f8eece9dbd0b86facb86011c666


Parameter | Value
------------ | ------------
symbol | BTC_USDT
side | BUY
type | LIMIT
quantity | 10
price | 6830


### Example 1: As a query string
* **Sort the query string components by byte order.:** price=6830&quantity=10&symbol=BTC_USDT&side=BUY&type=LIMIT
* **HMAC SHA512 signature:** Calculate the Signature:
The signature is calculated with the query string and secret key as inputs to a keyed hash function.
* Add the resulting value to the query header as a Signature parameter. 




### Pairs information
```
GET /v1/symbols
```
Current exchange  symbol information


**Parameters:**
NONE

**Response:**
```javascript
{
    "data":[
        {
            "price_precision":6,
            "amount_precision":4,
            "base":"ETH",
            "symbol":"eth_btc",
            "fee":"0.001",
            "quote":"BTC"
        }
    ],
    "status":"ok"
}
```

## Market Data endpoints
### Order book
```
GET /v1/orderBook
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |


**Response:**
```javascript
{
    "asks":[
        {
            "price":"7902",
            "volume":"1.98"
        }
    ],
    "bids":[
        {
            "price":"8011",
            "volume":"0.8"
        }
    ],
    "ts":1541684184,
    "symbol":"btc_usdt",
    "status":"ok"
}
```

### Recent trades
```
GET /v1/trades
```
Get recent trades (up to last 500).

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |

**Response:**
```javascript


```


### 24H ticker statistics
```
GET /v1/tickers
```

**Parameters:**
NONE

**Response:**
```javascript
{
    "status":"ok",
    "data":[
        {
            "low":"0.00010000",
            "changeRate":"0.00",
            "high":"377547814.62904483",
            "symbol":"eth_btc",
            "last":"3.00000000",
            "vol":"1335.0000",
            "open":"0.00000000"
        }
    ]
}
```


### Symbol ticker
```
GET /v1/ticker
```
Latest price for a symbol

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |



**Response:**
```javascript
{
    "status":"ok",
    "ticker":{
        "last":"10.000000",
        "vol":"71.000000",
        "high":"40.000000",
        "open":"0.000000",
        "changeRate":"0.000000",
        "low":"10.000000"
    },
    "ts":1541684558.181
}
```


## Trade or Account endpoints
### New order  (TRADE)
```
POST /v1/order  (HMAC signature)
```
Send in a new order.

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
side | ENUM | YES |
type | ENUM | NO |
quantity | DECIMAL | YES |
price | DECIMAL | YES |

Additional mandatory parameters based on `type`:

Type | Additional mandatory parameters
------------ | ------------
`LIMIT` |  `quantity`, `price`


**Response:**
```javascript

```

### Cancel order
```
DELETE /v1/orderCancel  (HMAC signature)
```
Cancel an open order.


**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
order_id | STRING | YES |


**Response:**
```javascript

```

### Current open orders 
```
GET /v1/openOrders  (HMAC signature)
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
page | STRING | NO |

* 50 records per page, page parameters starting from 0

**Response:**
```javascript
{
    "err_code":0,
    "err_msg":"OK",
    "total":10,
    "next_page":false,
    "list":[
        {
            "order_price":"56.0000000000",
            "price":"56.0000000000",
            "volume":"34.00000000",
            "remain":"34.00000000",
            "amount":"0.00000000",
            "type":"buy",
            "timestamp":1541681493,
            "order_id":"1060499209679016099",
            "market":"PXG_BTC",
            "status":0
        },
        {
            "order_price":"10.0000000000",
            "price":"10.0000000000",
            "volume":"1.00000000",
            "remain":"1.00000000",
            "amount":"0.00000000",
            "type":"buy",
            "timestamp":1541681476,
            "order_id":"1060499209679016098",
            "market":"PXG_BTC",
            "status":0
        }
    ]
}
```

### All orders (USER_DATA)
```
GET /v1/orderHistory  (HMAC signature)
```
Get all order hisroty (filled or canceled).


**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
page | STRING | NO |



**Response:**
```javascript

```

### Account information (USER_DATA)
```
GET /v1/account  (HMAC signature)
```
Get current account information.


**Parameters:**
NONE

**Response:**
```javascript
{
    "err_code":0,
    "err_msg":"ok",
    "data":[
        {
            "currency":"USDT",
            "balance":"112118195.47738258",
            "locked":"15026.00000000"
        },
        {
            "currency":"BTC",
            "balance":"106540000.39445214",
            "locked":"2205.66000000"
        },
        {
            "currency":"ETH",
            "balance":"95867066.03966032",
            "locked":"0.00000000"
        }
    ]
}
```



