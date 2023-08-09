<!-- START_METADATA
---
title: Quick start for the Order Management API
sidebar_label: Quick start
sidebar_position: 10
description: Quick steps for getting started with the Order Management API.
toc_min_heading_level: 2
toc_max_heading_level: 5
pagination_next: null
pagination_prev: null
---

import ApiSchema from '@theme/ApiSchema';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';


END_METADATA -->

# Quick start

Use the Order Management API to generate enriched receipts.

## Before you begin

This document covers the quick steps for getting started with the Order Management API.
You must have already signed up as an organization with Vipps MobilePay and have
your test credentials from the merchant portal, as described in the
[Getting started guide](https://developer.vippsmobilepay.com/docs/getting-started).

**Important:** The examples use standard example values that you must change to
use *your* values. This includes API keys, HTTP headers, reference, etc.

## Your first receipt

### Step 1 - Setup

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

**Please note:** Postman is discontinuing their offline version. Use only your test keys and delete them after testing. Ensure that your company allows for cloud use before continuing.

If you wish to use Postman, import the following files:

* [Order Management API Postman collection](/tools/vipps-order-management-api-postman-collection.json)
* [API Global Postman environment](https://raw.githubusercontent.com/vippsas/vipps-developers/master/tools/vipps-api-global-postman-environment.json)

In Postman, tweak the environment with your own values (see
[API keys](https://developer.vippsmobilepay.com/docs/common-topics/api-keys/)):

* `client_id` - Merchant key required for getting the access token.
* `client_secret` - Merchant key required for getting the access token.
* `Ocp-Apim-Subscription-Key` - Merchant subscription key.
* `merchantSerialNumber` - Merchant ID.
* `MobileNumber` - The phone number for the test app profile you have received or registered. This is your test mobile number *without* country code.

</TabItem>
<TabItem value="curl">

No setup needed :)

</TabItem>
</Tabs>

### Step 2 - Authentication

Get an `access_token` from the
[Access token API](https://developer.vippsmobilepay.com/docs/APIs/access-token-api):
[`POST:/accesstoken/get`][access-token-endpoint].

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

```bash
Send request Get Access Token
```

</TabItem>
<TabItem value="curl">

```bash
curl https://apitest.vipps.no/accessToken/get \
-H "client_id: YOUR-CLIENT-ID" \
-H "client_secret: YOUR-CLIENT-SECRET" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-H "Merchant-Serial-Number: 123456" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
-X POST \
--data ''
```

</TabItem>
</Tabs>

The property `access_token` should be used for all other API requests in the `Authorization` header as the Bearer token.

### Step 3 - Request a payment

Create a payment with either the ePayment API, eCom API, or Recurring API.

Provide unique values for `Idempotency-Key` and `orderId` each time you request a payment.

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

```bash
Send request Create Payment with ePayment API
```

</TabItem>
<TabItem value="curl">

```bash
curl --location 'https://apitest.vipps.no/ecomm/v2/payments/' \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Merchant-Serial-Number: 123456" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
-X POST \
-d '{
  "customerInfo": {
    "mobileNumber": "91234567"
  },
  "merchantInfo": {
    "merchantSerialNumber": "123456",
    "callbackPrefix":"https://example.com/vipps/callbacks-for-payment-update-from-vipps",
    "fallBack": "https://example.com/vipps/fallback-result-page-for-both-success-and-failure/acme-shop-123-order123abc",
  },
  "transaction": {
    "amount": 49900,
    "orderId": "PAYMENT-ORDER-ID",
    "transactionText": "One pair of socks.",
}
}'
```

</TabItem>
</Tabs>

Note that you can also create a receipt directly in the [ePayment API `createPayment` request](https://developer.vippsmobilepay.com/api/epayment/#tag/CreatePayments/operation/createPayment), but to add a category and image, you will use the steps below.

**Please note:**
The payment doesn't need to exist yet. It is possible to add the receipt first, then create the payment request afterwards.

### Step 4 - (Optional) Upload an image

Upload an image with: [`POST:/order-management/v1/images`][post-image-endpoint].
This image can be used in one or many receipts.

Provide an image in base64 format. Specify a unique `imageId`.

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

```bash
Send request Upload an image
```

</TabItem>
<TabItem value="curl">

```bash
curl https://apitest.vipps.no/order-management/v1/images \
-H "Content-Type: application/json" \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Merchant-Serial-Number: 123456" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
-X POST \
-d '{
  "imageId": "logo-12345678",
  "src": "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",
  "type": "base64"
}'
```

</TabItem>
</Tabs>

You can see the image in the payment history of the app, once you create a category and attach it to the payment.

### Step 6 - Add category to an order

Set the category, image, and order details URL by using
[`PUT:{paymentType}/categories/{orderId}`][put-category-endpoint].

Use `ecom` for the `paymentType` of ePayment or eCom payments. Use `recurring` for recurring payments.
For `orderId`, use the `orderId` or `reference` (for ePayment) of the payment.

The category is mutable, and a new request will completely overwrite previous requests.

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

```bash
Send request Add category to an order
```

</TabItem>
<TabItem value="curl">

```bash
curl https://apitest.vipps.no/order-management/v2/ecom/categories/PAYMENT-ORDER-ID \
-H "Content-Type: application/json" \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Merchant-Serial-Number: 123456" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
-X PUT \
-d '{
   "category": "GENERAL",
   "orderDetailsUrl": "https://www.example.com/2486791691483852025",
   "imageId": "logo-12345678"
}'
```

</TabItem>
</Tabs>

See [API guide: Categories](vipps-order-management-api.md#categories) for details.

### Step 7 - Add receipt to the payment

Set all details about the receipt with:
[`POST:{paymentType}/receipts/{orderId}`][post-receipt-endpoint].

A receipt is immutable and, once sent, cannot be overwritten.
So, if you want to run this example more than once, you'll need to create a new payment request to attach it to.

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

```bash
Send request Add receipt to an order
```

</TabItem>
<TabItem value="curl">

```bash
curl https://apitest.vipps.no/order-management/v2/ecom/receipts/PAYMENT-ORDER-ID \
-H "Content-Type: application/json" \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Merchant-Serial-Number: 123456" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
-X POST \
-d '{
  "orderLines": [
    {
      "name": "Socks",
      "id": "socks_123456789",
      "totalAmount": 1000,
      "totalAmountExcludingTax": 800,
      "totalTaxAmount": 200,
      "taxPercentage": 25,
    },
  ],
  "bottomLine": {
    "currency": "NOK",
  }
}'
```

</TabItem>
</Tabs>

See [API guide: Receipts](vipps-order-management-api.md#adding-a-receipt) for more details.

### Step 8 - (Optional) Fetch the receipt

Fetch the details stored about the order and the receipt by using
[`GET:{paymentType}/{orderId}`][get-order-endpoint].

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

```bash
Send request Get order with category and receipt
```

</TabItem>
<TabItem value="curl">

```bash
curl https://apitest.vipps.no/order-management/v2/ecom/payment12345 \
-H "Content-Type: application/json" \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Merchant-Serial-Number: 123456" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-H "Vipps-System-Plugin-Name: acme-webshop" \
-H "Vipps-System-Plugin-Version: 4.5.6" \
-X GET \
-d ''
```

</TabItem>
</Tabs>

## Next Steps

Visit the [Order Management API Guide](vipps-order-management-api.md) to read about the concepts and details.


[post-image-endpoint]: https://developer.vippsmobilepay.com/api/order-management#tag/Image/operation/postImage
[put-category-endpoint]: https://developer.vippsmobilepay.com/api/order-management#tag/Category/operation/putCategoryV2
[post-receipt-endpoint]: https://developer.vippsmobilepay.com/api/order-management#tag/Receipt/operation/postReceiptV2
[get-order-endpoint]: https://developer.vippsmobilepay.com/api/order-management#tag/Order/operation/getOrderV2
[access-token-endpoint]: https://developer.vippsmobilepay.com/api/access-token#tag/Authorization-Service/operation/fetchAuthorizationTokenUsingPost
[ecom-create-payment-endpoint]: https://developer.vippsmobilepay.com/api/ecom/#tag/Vipps-eCom-API/operation/initiatePaymentV3UsingPOST