# Quantum Exchange REST API Documentation

## API Endpoint

  `https://api.quantum.exchange`

All API requests should use the "application/json" content type.

## HTTP return codes

- 200 Success
- 4xx For malformed requests (the issue is on the client side)
- 5xx Internal errors (the issue is on the server side)

## Order Status

- PENDING
- OPEN
- PARTIAL
- COMPLETED
- CANCELLED
- REJECTED

## Order Types

- LIMIT
- MARKET

## Order Options

- *fill-or-kill* : Order should complete fully immediately or cancel. Will not add liquidity to the order book.
- *make-or-cancel* : If any part of this order could be fullfilled the order will cancel. This order only adds liquidity to the order book. Only applies to limit orders. (Like Poloniex's "postOnly")
- *inmediate-or-cancel* : Any portion not filled will be canceled. Will not add liquidity to the order book.

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
      "Id":"xxxxx"
   }
   ``` 

* **Response fields**

  **Id** : Order ID (Guid).  

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
     { "id": "cad", "balance":20000.0 },
     { "id": "btc", "balance":21.17839 },
     { "id": "ltc", "balance":100.05 }
   }
   ```  

* **Response fields**

  **Id** : Order ID (Guid).  
  
### Get Open Orders

* **URL** 
  
  /v1/orders/open

* **Method:**

  `GET`

### Lookup Order

* **URL** 
   
  /v1/orders/{orderId}

* **Method:**

  `GET`

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
  **type** : LIMIT | MARKET  
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

  { "id" : "xxxxxxx"  }

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
  
### Cancel Order

* **URL** 
  
  /v1/order/cancel/all

* **Method:**

  `POST`
  
* **Authentication:**

  *Required*

* **Data Params**

  { "asset" : "BTC"; currency: "USDT"  }

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
  
  /v1/crypto/withdraw

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

### Withdraw Fiat

* **URL** 
  
  /v1/fiat/withdraw

* **Method:**

  `POST`
  
* **Authentication:**

  *Required*

* **Data Params**

  { 
    "amount" : 120000.0,
    "currency"  : "CAD"
  }

  ### Required fields.

  **amount** : Amount of fiat  
  **currency** : Currency  

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
  
  
