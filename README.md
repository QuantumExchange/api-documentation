# Quantum Exchange REST API Documentation

## API Endpoint

  `https://api.quantum.exchange`

All API requests should use the "application/json" content type, and must be encrypted with SSL (https).

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

### Place Limit Order

* **URL** 
  
  /v1/create_limit_order

* **Method:**

  `POST`
  
* **Authentication:**

  *Required*

* **Data Params**

  { "action" : "buy" ; "amount" : 14.2; "asset" : "btc"; "currency" : "cad"; "price" : 9009.24 }

  ### Required fields.

  **action** : Buy or Sell  
  **amount** : Amount.  
  **asset** : Asset.  
  **currency** : Currency (CAD).  
  **price** : Limit price  

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

### Place Market Order

* **URL** 
  
  /v1/create_market_order

* **Method:**

  `POST`
  
* **Authentication:**

  *Required*

* **Data Params**

  { "action" : "buy" ; "amount" : 14.2; "asset" : "btc"; "currency" : "cad" }

  ### Required fields.

  **action** : Buy or Sell
  **amount** : Amount.  
  **asset** : Asset.
  **currency** : Currency (CAD).  

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
  
  /v1/cancel_order

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
    "asset"  : "ada"
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
    "currency"  : "cad"
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
  
  
