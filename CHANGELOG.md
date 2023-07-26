<!-- START_METADATA
---
title: Order Management API changelog
sidebar_label: Changelog
sidebar_position: 400
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Changelog

All notable changes to the current API will be documented in this file.
To learn about API versioning, see
[Common topics: API Lifecycle](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/api-lifecycle/).

## March 2023

Added `receiptNumber` and valid barcode types.

## January 2023

* Added barcode data validation.
* On `bottomLine` of the receipt, replaced `receiptNumber` with `barcode` and added `posId`.
* Added kilowatt-hour (Kwh) as quantity unit on orderlines.
* Added `receiptNumber` to the `bottomLine` of the receipt.


## December 2022

In order to be a valid electronic sales receipt, we added `paymentSources` to the receipt `bottomLine`:

* card
* voucher
* cash
* giftcard

## August 2022

Added `IsReturn` and `isShipping` to `orderLines`.

* Shipping lines are now treated as regular `orderLines`.
* Removed validation of sum, tax and discount in the bottom lines.
* `IsReturn` flag is now added to `orderLines`. A returned `orderLine` will be treated as negative value.
* Removed possibility of having negative amount on `orderLines`.

## June 2022

Added `shippingInfo` object to the receipt.

## September 2021

An early version of the Order Management API is made available.
