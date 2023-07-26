<!-- START_METADATA
---
title: Quick start for the Order Management API
sidebar_label: Quick start
sidebar_position: 10
description: Quick start guide for the using the Order Management API with Postman.
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Quick start

Use the Order Management API to generate enriched receipts.

<!-- START_COMMENT -->

ℹ️ Please use the new documentation:
[Vipps MobilePay Technical Documentation](https://developer.vippsmobilepay.com/docs/APIs/order-management-api).

## Table of Contents

* [Quick start](#quick-start)
  * [Table of Contents](#table-of-contents)
  * [Postman](#postman)
    * [Prerequisites](#prerequisites)
    * [Step 1: Get the Postman collection and environment](#step-1-get-the-postman-collection-and-environment)
    * [Step 2: Import the Postman files](#step-2-import-the-postman-files)
    * [Step 3: Set up Postman environment](#step-3-set-up-postman-environment)
  * [Make API calls](#make-api-calls)

<!-- END_COMMENT -->

## Postman

### Prerequisites

Review
[Vipps quick start guides](https://developer.vippsmobilepay.com/docs/vipps-developers/quick-start-guides)
for information about getting your test environment set up.

### Step 1: Get the Postman collection and environment

Save the following files to your computer:

* [Order Management API Postman collection](/tools/vipps-order-management-api-postman-collection.json)
* [API Global Postman environment](https://raw.githubusercontent.com/vippsas/vipps-developers/master/tools/vipps-api-global-postman-environment.json)

### Step 2: Import the Postman files

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
   * `merchantSerialNumber` - Merchant ID.
   * `mobileNumber` - The mobile number for the test app profile you have received or registered.

## Make API calls

For all the following, you will start by sending request `Get Access Token` which uses
[Get Access Token][access-token-endpoint].
This provides you with access to the API.

The access token is valid for 1 hour in the test environment
and 24 hours in the production environment.
See the
[API reference][order-mgmt-api-reference-url]
for details about the calls.

### Add receipt with order info and image

1. Send request `Get Access Token`. This provides you with access to the API.

1. Create an order.
   In this example, we will create an eCom order by sending request `Initiate Payment`.
   This generates a one-time payment by using
   [`POST:/ecomm/v2/payments`][ecom-initiate-payment-endpoint]
    and sets an `order_Id` in environment for usage in subsequent steps.

   The response will be a URL to the landing page.
   *Ctrl+click* (*Command-click* on macOS) on the link that appears, and it will take
   you to the [landing page](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/landing-page).
   The phone number of your test user should already be filled in, so you only have to click "Next".

   You have now confirmed the payment in Vipps, setting the payment status to reserved.

   **Note:**
   The order doesn't need to exist yet. It is possible to add the receipt first, then create the order afterwards.

1. Send request `Add an image to an order`. This creates an image that can be used in one or many receipts by using
   [`POST:/order-management/v1/images`][add-image-endpoint].

1. Send request `Add category to an order`. This sets the category, image, and order details URL by using
   [`PUT:/order-management/v2/{{paymentType}}/categories/{{orderId}}`][add-category-endpoint].
   In this and the following steps, we use the `paymentType` of `eCom` and the `orderId` that was set in the `Initiate Payment` request.

1. Send request `Add receipt to an order`. This sets all details about the receipt by using
   [`POST:/order-management/v2/{{paymentType}}/receipts/{{orderId}}`][add-receipt-endpoint].

1. Send request `Get order with category and receipt`. This gets all details stored about the order and the receipt by using
   [`GET:/order-management/v2/{{paymentType}}/{{orderId}}`][get-order-endpoint].

[order-mgmt-api-reference-url]: https://developer.vippsmobilepay.com/api/order-management
[add-image-endpoint]: https://developer.vippsmobilepay.com/api/order-management#tag/Image/operation/postImage
[add-category-endpoint]: https://developer.vippsmobilepay.com/api/order-management#tag/Category/operation/putCategoryV2
[add-receipt-endpoint]: https://developer.vippsmobilepay.com/api/order-management#tag/Receipt/operation/postReceiptV2
[get-order-endpoint]: https://developer.vippsmobilepay.com/api/order-management#tag/Order/operation/getOrderV2
[access-token-endpoint]: https://developer.vippsmobilepay.com/api/access-token#tag/Authorization-Service/operation/fetchAuthorizationTokenUsingPost
[portal-url]: https://portal.vipps.no
[ecom-initiate-payment-endpoint]: https://developer.vippsmobilepay.com/api/ecom#tag/Vipps-eCom-API/operation/initiatePaymentV3UsingPOST
