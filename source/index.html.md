---
title: Athena Retail API Reference

language_tabs:
  - curl
  - json

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - retail_trans
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
