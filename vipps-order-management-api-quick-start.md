<!-- START_METADATA
---
title: Quick start
sidebar_position: 20
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Quick start

<!-- START_COMMENT -->

ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).

<!-- END_COMMENT -->

Use the Order Management API to generate enriched receipts.

<!-- START_TOC -->

## Table of Contents

- [Quick start](#quick-start)
  - [Table of Contents](#table-of-contents)
  - [Postman](#postman)
    - [Prerequisites](#prerequisites)
    - [Step 1: Get the Vipps Postman collection and environment](#step-1-get-the-vipps-postman-collection-and-environment)
    - [Step 2: Import the Vipps Postman files](#step-2-import-the-vipps-postman-files)
    - [Step 3: Set up Postman environment](#step-3-set-up-postman-environment)
  - [Make API calls](#make-api-calls)
    - [Add receipt with order info and image](#add-receipt-with-order-info-and-image)
  - [Questions?](#questions)

<!-- END_TOC -->

Document version 1.1.2.

## Postman

### Prerequisites

Review
[Vipps quick start guides](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/quick-start-guides)
for information about getting your test environment set up.

### Step 1: Get the Vipps Postman collection and environment

Save the following files to your computer:

* [Vipps Order Management API Postman collection](tools/vipps-order-management-api-postman-collection.json)
* [Vipps API Global Postman environment](https://raw.githubusercontent.com/vippsas/vipps-developers/master/tools/vipps-api-global-postman-environment.json)

### Step 2: Import the Vipps Postman files

1. In Postman, click *Import* in the upper-left corner.
1. In the dialog that opens, with *File* selected, click *Upload Files*.
1. Select the two files you have just downloaded and click *Import*.

### Step 3: Set up Postman environment

1. Click the down arrow, next to the "eye" icon in the top-right corner, and select the environment you have imported.
2. Click the "eye" icon and, in the dropdown window, click `Edit` in the top-right corner.
3. Fill in the `Current Value` for the following fields to get started. For the first three keys, go to *Vipps Portal* > *Utvikler* ->  *Test Keys*.
   * `client_id` - Merchant key is required for getting the access token.
   * `client_secret` - Merchant key is required for getting the access token.
   * `Ocp-Apim-Subscription-Key` - Merchant subscription key.
   * `merchantSerialNumber` - Merchant id.
   * `mobileNumber` - The mobile number for the test app profile you have received or registered.

## Make API calls

For all of the following, you will start by sending request [Get Access Token](https://vippsas.github.io/vipps-developer-docs/api/ecom#tag/Authorization-Service/operation/fetchAuthorizationTokenUsingPost).
This provides you with access to the API.

The access token is valid for 1 hour in the test environment
and 24 hours in the production environment.  
See the
[API reference](https://vippsas.github.io/vipps-developer-docs/api/order-management)
for details about the calls.

### Add receipt with order info and image

1. Send request [Initiate Payment](https://vippsas.github.io/vipps-developer-docs/api/ecom#tag/Vipps-eCom-API/operation/initiatePaymentV3UsingPOST).
   This generates an One Time Payment and sets an `order_Id` in environment for usage in pt. 2 to 5.

2. Send request [Add an image to an order](https://vippsas.github.io/vipps-developer-docs/api/order-management#tag/Image/operation/postImage).
   This creates an image that can be used in one or many receipts.

3. Send request [Add category to an order](https://vippsas.github.io/vipps-developer-docs/api/order-management#tag/Category/operation/putCategoryV2).
   This sets the category, image and 
order details url, where merchants can link to external order information details.

4. Send request [Add receipt to an order](https://vippsas.github.io/vipps-developer-docs/api/order-management#tag/Receipt/operation/postReceiptV2).
   This sets all details about the receipt.

5. Send request [Get order with category and receipt](https://vippsas.github.io/vipps-developer-docs/api/order-management#tag/Order/operation/getOrderV2).
   This gets all details stored about the order and the receipt.

See the [Order Management API Specifications](https://vippsas.github.io/vipps-developer-docs/api/order-management) for details about the calls.


## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-qr-api/issues),
a [pull request](https://github.com/vippsas/vipps-order-management-api/pulls),
or [contact us](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/contact).

Sign up for our [Technical newsletter for developers](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/newsletters).
