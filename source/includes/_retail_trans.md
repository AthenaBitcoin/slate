# Retail Transactions

The "guts" of the API is the Retail Transaction.  The retail transaction is the key resource that you will use when making a purchase with Athena.    Through this resource, you'll l update the status of the transaction and be able to be updated about the progress from initiation to completion.

## Initiate a Transaction

### HTTP Request

`POST https://api.athenabitcoin.com/v1/retail_transaction/initiate`

```curl

curl https://api.athenabitcoin.com/v1/retail_transaction/initiate
   -X POST
   -H 'Content-Type: applicaton/json'
   -H 'Authorization: Basic apikey'
   -d '{"request_amount": "1000.00",
        "clordid": "abcdef",
        "auth_token": "xyz"}
```

### Post Parameters

Parameter | Default | Description
--------- | ------- | -----------
request\_amount | 1000 | The amount, in US Dollars, the customer would like to purchase.  This parameter may affect the price, how long the quote (the price) we offer is valid for & what identification will be needed in order to complete the transaction.
clordid | - | *Required* A string that identifies this transaction.  Must be unique for this auth_token.
request\_currency | USD | **Ignored** Reserved for multicurrency

<aside class="success">
On Success, 200, there will be a json response</aside>

```json
{
  "data": {

    "clordid": "trans-uuid",
    "authorized_amount": 50,
    "vending_price": 484.30,
    "authorization_expires_utc": "2016-04-15T23:59",
    "order_status": "A",
    "required_info": [
          "cell_number",
          ...
       ]
  }
}
```

Parameter | Description
----------|------------
clordid   | Echoing back what was sent
authorized_amount | An amount equal to, *or less than* the amount requested.  This is the maximum amount, in US dollars, that we can sell for this transaction.
vending_price | The price at which we are offering to sell.  We may quote prices to the one hundred millionth of a Bitcoin, a [Satoshi](https://en.bitcoin.it/wiki/Satoshi_%28unit%29).
authorization_expires_utc | The time, in UTC in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format, when the transaction will expire.
order_status: The order status of the order.  See the [Order Status Codes](#OrdStatusCodes) for their meanings and descriptions.


## Provide Customer Information

### HTTP Request

`POST https://api.athenabitcoin.com/v1/retail_transaction/info_submit`

### Post Parameters

```json
[
{
"clordid": "trans-uuid",
"postnum":"1",
"complete": false,
"data-key": "sms_tel",
"cust-data": "+1 452993xxxx"
},
]
```

As more data becomes from the customer, it can be submitted via sucessive submissions.  One would call info_submit with additional data.


(Info Parameter)[#required-info] | Description
--------- | -----------
Parameter | provided information (see table for how data is to be submitted.)


## Receive Address

This endpoint is intended for the user to submit the address where they would like the Bitcoin sent.

### HTTP Request

`POST https://api.athenabitcoin.com/v1/retail_transaction/address_submit`

### Parameters

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
clordid   | True     | N/A     | Client Order Id (ClOrdId) Associated with this order |
addr | True     | N/A     | The bitcoin address where the bitcoin should be sent |

## Payment Received

As the merchant receives payment for a Bitcoin, or Bitcoin loaded gift card, sale the merchant may elect to provide incremental updates to Athena.  Only once athena acknowledges and affirms the money was received will we consumate the trade.

One instance where a merchant may use this feature is when a customer would like to purchase $100 in Bitcoin.  After [Initiating](#Initiate a Transaction) the transaction and receiving the appropriate authorization, the customer realizes rather than 5 $20 bills, they have 4.  The merchant may notify Athena $80 was received, and the customer can either dig in their pocket more - or go on to complete the transaction.

Another instance where this feature may be used is if the merchant integrates with a bill scanner.  In that case, the merchant may notify Athena whenever a bill is entered is accepted into the bill acceptor.  Had the customer submitted 4 bills in this way, 4 separate calls to this endpoint could be made.

### HTTP Request

`POST https://api.athenabitcoin.com/v1/retail_transaction/payment_recv`

### Parameters

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
clordid   | True     | N/A     | Client Order Id (ClOrdId) Associated with this order |
recv\_amount | True     | N/A     | The amount the merchant has received for this transaction. |
recv\_currecy| False | USD | The curency; *Reserved for Multi-currency Support.*
Values other than USD will be rejected.

## Complete a Transaction

The endpoint to be called once all steps of the process are complete.  The result of this transaction will be to issue the customer both an Athena Transaction Confirmation and valid Transaction Id for the Bitcoin Blockchain.  (Using this transaction id, the user can independently confirm the bitcoin have been sent to the wallet address they provided.)

### HTTP Request

`POST https://api.athenabitcoin.com/v1/retail_transaction/complete`

### Parameters

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
clordid   | True     | N/A     | Client Order Id (clOrdId) Associated with this order |
total_recv | True | N/A | The total amount received as part of this transaction
recv\_currecy| False | USD | The curency; *Reserved for Multi-currency Support.*
Values other than USD will be rejected.
