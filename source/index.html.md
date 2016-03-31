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

Welcome to the [Athena Bitcoin](http://www.athenabitcoin.com) retail API.  Using this API, you'll be able to offer customer's the opportunity to purchase Bitcoin.  If you're new with Bitcoin, there are numerous places online where you can read up on the technology.  Coin Desk's [Introduction to Bitcoin](http://www.coindesk.com/information/) is a great place to get started.

This API, and associated endpoints is intended, to allow a merchant to integrate selling Bitcoin, or Gift Cards denominated in Bitcoin, into their Point of Sale system.  This is API is *not* intended for trading.

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

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve
