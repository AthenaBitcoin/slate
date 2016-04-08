---
title: Athena Retail API Reference

language_tabs:
  - json

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors
  - required_info
  - transaction_status

search: true
---

# Introduction

Welcome to the [Athena Bitcoin](http://www.athenabitcoin.com) retail API.  Using this API, you'll be able to offer customer's the opportunity to purchase Bitcoin.  If you're new with Bitcoin, there are numerous places online where you can read up on the technology.  Coin Desk's [Introduction to Bitcoin](http://www.coindesk.com/information/what-is-bitcoin/) is a great place to get started.

This API, and associated endpoints, is intended to allow a merchant to integrate selling gift cards denominated in Bitcoin, at their Point of Sale system.  This is API is *not* intended for trading.

Why is a special API necessary?  And why can you just not sell gift cards loaded with Bitcoin as ordinary gift cards... or put another way: why is this API necessary?  In the US Bitcoin is classified by FinCen as a "[convertible virtual currency](https://www.fincen.gov/statutes_regs/guidance/html/FIN-2013-G001.html)."  The effect of that is businesses selling Bitcoin, like Athena, have a number of regulatory obligations that sellers of regular gift cards do not have.  This API is intended to address those obligations.  Additionally, the value of Bitcoin fluctates compared to other currencies.  Through this API, users can see what at what price Athena is able to sell Bitcoin.
  
## Version

This is Version 1 (v1) of the API.  Feedback is enouraged and appreciated.

## Wallets

In order to use bitcoin, customers will need to store their Bitcoin in a wallet.  While customers can use any bitcoin wallet, Athena has both a [iOS](http://bit.ly/BTCWalletAppStore) and an [Android](http://bit.ly/BTCWalletAndroid) wallet.  We encourage users use the Athena Wallet.

We have language bindings in Shell, and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](https://github.com/tripit/slate). Feel free to edit it and use it as
 a base for your own API's documentation.

# Authentication

Authentication for the API requires an API Key.  All tokens are tied to a business entity (a Firm).  The firm is responsible for making sure proceeds from any sales are sent to Athena.  Firms can have multiple points of sale.  A point of sale will be the terminal from which a transaction can occur.  _API Keys are assigned at the Point of Sale_ level.

Once the firm has the appropriate paper work in place, just let us know, how many terminals Bitcoin can be sold from. We'll also need to know physical location for each point of sale.

For now you can write [development@athenabitcoin.com](mailto:development@athenabitcoin.com) to request a key.

# Retail Transactions

The "guts" of the API is the Retail Transaction.  The retail transaction is the key resource that you will use when making a purchase with Athena.    Through this resource, you'll l update the status of the transaction and be able to be updated about the progress from initiation to completion.

## Initiate a Transaction

### HTTP Request

`POST https://api.athenabitcoin.com/v1/retail_transaction/initiate`

### Post Parameters

Parameter | Default | Description
--------- | ------- | -----------
auth_token | - | *Required* parameter to describe where the transaction is occuring.
request_amount | 1000 | The amount, in US Dollars, the customer would like to purchase.  This parameter may affect the price, how long the quote (the price) we offer is valid for & what identification will be needed in order to complete the transaction.
clordid | - | *Required* A string that identifies this transaction.  Must be unique for this auth_token.
request_time_utc | - | *Required* The time at which the request is being made.

<aside class="success">
On Success, 200, there will be a json response that has paremeters like the following
</aside>


> If the request succeeds, the user will receive JSON like the following:

```json
[
  {
    "clordid": "trans-uuid",
    "authorized_amount": 50,
    "vending_price": 484.30,
    "authorization_expires_utc": "2016-04-15T23:59",
    "order_status": 'A',
    "required_info": [
          "cell_number",
          ...
       ]
  }
]
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

As more data becomes from the customer, it can be submitted via sucessive submissions.  One would


(Info Parameter)[#required-info] | Description
--------- | -----------
Parameter | provided information (see table for how data is to be submitted.)
