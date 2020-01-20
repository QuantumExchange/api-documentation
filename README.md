# Quantum Exchange REST API Documentation

## API Endpoint

  `https://api.quantum.exchange`

All API requests should use the "application/json" content type.

## Endpoint limits

Maximum six (6) requests per second. When exceeded the endpoint will answer with status code 429.

## HTTP return codes

- 200 Success
- 4xx For malformed requests (the issue is on the client side)
- 5xx Internal errors (the issue is on the server side)

## Order Status

- PENDING
- OPEN
- COMPLETED
- CANCELLED
- REJECTED

## Order Types

- LIMIT
- MARKET
- STOP-LOSS-LIMIT
- STOP-LOSS-MARKET
- TAKE-PROFIT-LIMIT
- TAKE-PROFIT-MARKET

## Order Options

- *fill-or-kill* : Order should complete fully immediately or cancel. Will not add liquidity to the order book.
- *make-or-cancel* : If any part of this order could be fullfilled the order will cancel. This order only adds liquidity to the order book. Only applies to limit orders. (Like Poloniex's "postOnly")
- *immediate-or-cancel* : Any portion not filled immediately will be canceled. Will not add liquidity to the order book.

## Public endpoints

### Get Order Book

* **URL**

  /v1/order_book/{asset}/{currency}

* **Method:**

  `GET`

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
```json
{
  "asset":"eth",
  "currency:"dai",
  "bids":[
     {
    "id": "a63b160e-8b37-4255-8ea0-4b3fbf7cbb87",
    "amount": 0.18897671,
    "filled": 0,
    "price": 8726.37760913
    }
  ],
  "asks":[],
  "timestamp":"2019-01-14 22:11:55"
}
```  

* **Response fields**

  **asset** : Asset symbol  
  **currency** : Currency symbol  
  **bids** : Array of bid orders  
  **asks** : Array of ask orders  
  **timestamp** : Timestamp  

## Private endpoints

All private REST request should contain the following HTTP Headers.

 - QUANTUM_API_KEY : Api key  
 - QUANTUM_API_SIGNATURE : Signature of payload; sha256 HMAC (nonce + method + request_path + body)  
 - QUANTUM_API_NONCE : The nonce, a positive integer that must never repeat and must increase between requests.  

### Account Balance

* **URL**

  /v1/balance
  
* **Method:**

  `GET`

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
   ```
   {
     { "id": "btc", "balance": { "total" : 21.0000000, "available" : 19.0000000 } },
     { "id": "bch", "balance": { "total" : 600.0000000, "available" : 600.00000000 } },
     { "id": "eth", "balance": { "total" : 100.0000000, "available" : 100.0000000 } }
   }
   ```  

* **Response fields**

  **id** : Asset symbol.  
  **balance** : Dictionary containing total and available balance. Available equals the total minus the amount placed on trading orders.
  
### Get Open Orders

Get open orders returns an array containing the orders with status open.

* **URL** 
  
  /v1/orders/{asset}/{currency}/open

* **Method:**

  `GET`
  
* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
   ```
   [ {"id":"bafa8e52-8592-4ed6-b79a-5f937148ec12","action":"buy","amount":1.66134784,"filled":0.0,"price":8657.20671787}
    ,{"id":"be3b4449-a5d4-4de1-9f80-3a0aa116e69b","action":"buy","amount":0.83112433,"filled":0.0,"price":8656.85507901}, ... ]


### Place New Order

* **URL** 
  
  /v1/order/new

* **Method:**

  `POST`
  
* **Authentication:**

  *Required*

* **Data Params**

  { "action" : "BUY" ; "amount" : 14.2; "asset" : "ETH"; "currency" : "USDT"; "price" : 9009.24; "type" : "LIMIT" }

  ### Required fields.

  **action** : BUY | SELL   
  **amount** : Amount.  
  **asset** : Asset.  
  **currency** : Currency.  
  **price** : Limit price  
  **stop_price** : Stop price   
  **type** : "limit" | "market" | "stop-loss-limit" | "stop-loss-market" | "take-profit-limit" | "take-profit-market"  
  **options** : Array of options "fill-or-kill", "make-or-cancel", "inmediate-or-cancel"

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
   ```
   {
      "id":"xxxxxx"
   }
   ``` 

* **Response fields**

  **Id** : Order ID (Guid). 

### Cancel Order

* **URL** 
  
  /v1/order/cancel

* **Method:**

  `POST`
  
* **Authentication:**

  *Required*

* **Data Params**

  { "id" : "xxxxxxx"; "asset" : "eth"; "currency" : "usdt"  }

  ### Required fields.

  **id** : ID of order to cancel

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
   ```
   {
      "success":"true"
   }
   ``` 

* **Response fields**

  **success** : boolean
  
### Cancel All Orders

* **URL** 
  
  /v1/order/cancel/all

* **Method:**

  `POST`
  
* **Authentication:**

  *Required*

* **Data Params**

  { "asset" : "btc"; currency: "usdt" }

  ### Required fields.

  **id** : ID of order to cancel

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
   ```
   {
      "success":"true"
   }
   ``` 

* **Response fields**

  **success** : boolean  

### Withdraw Crypto

* **URL** 
  
  /v1/withdraw

* **Method:**

  `POST`
  
* **Authentication:**

  *Required*

* **Data Params**
  
  { 
    "amount" : 45.0,
    "asset"  : "ADA"
  }

  ### Required fields.

  **amount** : Amount of crypto  
  **asset** : Asset descriptor  

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
   ```
   {
      "success":"true"
   }
   ``` 

* **Response fields**

  **success** : boolean  

 
## Websocket Stream
  
  * **URL**
    /v1/order_book/stream
    
  * **EVENTS**
  
    - order_created
    - order_updated
    - order_cancelled
    - order_completed
    - new_trade
  
  
