<!-- START_METADATA
---
title: API Guide
sidebar_position: 12
---
END_METADATA -->

# Vipps Order Management API v2

The Order Management API allows merchants to enrich Vipps Transactions.
The information given in this API will be shown to the customer in the order
history in their app. Order Management operates with two concepts: [Categories](#categories) (with images) and [Receipts](#Receipts). These concepts may be used separately or combined, and this guide will explain how to add them.

Vipps Order Management enables you to communicate with your customers through
the payment receipts in the Vipps app. The purpose of doing this is to give
your customers more convenience, better overview and a more compelling shopping
experience when they use Vipps to pay for your products and services.
Vipps Order management also enables you to draw customers back to your website
or app from links on the Vipps receipt view.

API version: 2.3.0.

Document version: 1.1.2.

<!-- START_TOC -->
## Table of contents

- [Before you begin](#before-you-begin)
  - [Authentication](#authentication)
  - [Vipps HTTP headers](#vipps-http-headers)
  - [OrderId and PaymentType](#orderid-and-paymenttype)
  - [Basic flow](#basic-flow)
- [Categories](#categories)
  - [Category](#category)
  - [Images](#images)
  - [Adding an Image](#adding-an-image)
  - [Adding and changing Category](#adding-and-changing-category)
- [Receipts](#receipts)
  - [Adding a Receipt](#adding-a-receipt)
  - [Fetching Category and Receipt](#fetching-category-and-receipt)
- [Vipps Assisted Content Monitoring](#vipps-assisted-content-monitoring)
    - [Mandatory use case](#mandatory-use-case)
  - [Questions?](#questions)

<!-- END_TOC -->

## Before you begin

This document assumes you have signed up as a organization with Vipps and have
retrieved your API credentials for
[the Vipps test environment](https://github.com/vippsas/vipps-developers/blob/master/vipps-test-environment.md)
from
[portal.vipps.no](https://portal.vipps.no).

### Authentication

All Vipps API calls are authenticated and authorized with an access token
(JWT bearer token) and an API subscription key:

| Header Name | Header Value | Description |
| ----------- | ------------ | ----------- |
| `Authorization` | `Bearer <JWT access token>` | Type: Authorization token. This obtained as described in [Getting started](https://github.com/vippsas/vipps-developers/blob/master/vipps-getting-started.md): [Get an access token](https://github.com/vippsas/vipps-developers/blob/master/vipps-getting-started.md#get-an-access-token) |
| `Ocp-Apim-Subscription-Key` | Base 64 encoded string | The subscription key for this API. This is available on [portal.vipps.no](https://portal.vipps.no). |

For more information about how to obtain an access token and all details around this, please see:
[Quick overview of how to make an API call](https://github.com/vippsas/vipps-developers/blob/master/vipps-getting-started.md#quick-overview-of-how-to-make-an-api-call).

### Vipps HTTP headers

We recommend using the following (optional) HTTP headers for all requests to the
Vipps eCom API. These headers provide useful metadata about the merchant's system,
which help Vipps improve our services, and also help in investigating problems.

These headers are **required for plugins and partners** and sent by the recent versions of
[the official Vipps plugins](https://github.com/vippsas/vipps-developers#plugins)
and we recommend all customers with direct integration with the API to also do so.

Partners must always send the `Merchant-Serial-Number` header, and we recommend that
everyone sends it too. It can speed up any trouble-shooting quite a bit.

| Header                        | Description                                  | Example value       |
| ----------------------------- | -------------------------------------------- | ------------------- |
| `Merchant-Serial-Number`      | The MSN for the sale unit                    | `123456`            |
| `Vipps-System-Name`           | The name of the ecommerce solution           | `woocommerce`       |
| `Vipps-System-Version`        | The version number of the ecommerce solution | `5.4`               |
| `Vipps-System-Plugin-Name`    | The name of the ecommerce plugin             | `vipps-woocommerce` |
| `Vipps-System-Plugin-Version` | The version number of the ecommerce plugin   | `1.4.1`             |

### OrderId and PaymentType

The idea with order management is to add a Receipt or Category to a Vipps transaction made with the Ecom or Recurring API. So, the receipt needs to be connected to a `OrderId`. The `OrderId` is what **you** use when either initiating a Ecom payment or creating a recurring charge.

The Order Management API does **no validation if the order exists**. This means that the order management enrichment may be added before or after the payment is actually created. The preferred way is to add the Order Management Data simultaneously as you initiate the payment. 

As the same OrderId can be used for both a Recurring charge and a Ecom payment, you need to supply which Vipps Product is being used by setting the appropriate `paymentType`

### Basic flow

1. Initiate a Vipps eCom or recurring payment
    - `POST:/ecomm/v2/payments` 
2. Save the **orderId** you used when initiating
3. Add an image (optional)
    - `POST:/v1/images` 
4. Save the imageId
5. Add a category with link and image
    - `/v2/{paymentType}/categories/{orderId}`
6. Add a receipt 
    - `/v2/{paymentType}/receipts/{orderId}`


## Categories

* [API specification with examples](https://vippsas.github.io/vipps-order-management-api/redoc.html#tag/Category)


The following section will explain how to enrich a Vipps transaction with `Categories` and `Images`. `Link` and `Category` are required when using this API,
whereas `Images` are optional.

### Category

In order to provide customers with more up-to-date information about their order,
you can add a `Category`. This creates a link on the Vipps Transaction page of the Vipps app and can take the
customer to a location on your website. Links are activated when a customer
clicks the corresponding button in the transaction page in the Vipps App.

The mobile device's standard web browser will open and the user will be redirected to the link location. Below you can see an example of a Vipps transaction with the "Shipping information" `Category`.

!["Shipping information link"](images/order-link-shipping-information.png)

The category will determine how the app handles the link, additional information, and the push.
You can only use one category. If you send more than one, only the last one will be shown in the app.

We currently support these categories:

| Category                      | Description                                  | Example images      |
| ----------------------------- | -------------------------------------------- | ------------------- |
| `Receipt`                     | A link to a location where the customer can access and download a valid proof of purchase and receipt for this particular order.     | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ![show receipt example](images/show-receipt.png) ![show receipt example](images/show-receipt-img.png)|
| `Order Confirmation`          | A link to a location that contains information and status of the order. If your webshop or site has a "My orders" page or similar, this link category can take the customer there. | ![show order info example](images/show-order-info.png) ![show order info example](images/show-order-info-img.png)|
| `Delivery information`        | A link to a location that contains information and status about the shipping or delivery related to the order. This could be a link to a site hosted by your freight carrier, or a link to your site. If your webshop or site has a "My order" page that includes delivery related information about the order, this link category can be used. | ![show delivery example](images/track-your-order.png) ![show delivery example](images/track-your-order-img.png)|
| `Ticket`                      | A link to a location where the customer can access and download a ticket to an event, trip or transportation. | ![show ticket example](images/show-ticket.png) ![show ticket example](images/show-ticket-img.png)|
| `Booking`                     | A link to a location that contains information and status about a booking, such as travel and rental booking. If your webshop or site has a "My bookings" page or similar, this link category can take the customer there. | ![show receipt example](images/show-booking-info.png) ![show receipt example](images/show-booking-info-img.png)|
| `General`                     | If none of the other categories fit the use case for the link, a *General* category can be used. This is a link to a location that contains any kind of information pertinent to the order. | ![show more info example](images/show-more-info.png) ![show more info example](images/show-more-info-img.png)|




### Images

With Vipps Order Management API you can upload an Image that is shown on the
transaction in the Vipps app. When adding an `Image` along with a `Category`, the Image needs to already exist. This means that the Image needs to be uploaded first. After uploading a image to 
the [`POST:/images`](https://vippsas.github.io/vipps-order-management-api/#/images/postImage) endpoint, the newly uploaded
image's `ImageId` needs be used in the Categories API.

The same image may be used for multiple transactions, but uploading a unique image for each
transaction is also OK. Imaged are fetched authenticated from the app, so feel
free to add tickets and receipts as images. Below you can see an example of a Vipps
transaction containing an Image with the "Shipping information" `Category`.

!["Shipping information link"](images/order-link-shipping-information-with-image.png)

### Adding an Image

[`POST:/order-management/v1/images`](https://vippsas.github.io/vipps-order-management-api/#/images/postImage)

Endpoint for uploading pictures. Images exist independently of any transaction.
It is not possible to overwrite an image. Base64 is the only supported media type at the moment.
Example request:

Headers:

```text
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <snip>
Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a
Merchant-Serial-Number: 123456
Vipps-System-Name: Acme Enterprises Ecommerce DeLuxe
Vipps-System-Version: 3.1.2
Vipps-System-Plugin-Name: Point Of Sale Excellence
Vipps-System-Plugin-Version 4.5.6
```

Body:

```json
{
  "imageId": "vipps-socks-orange-123",
  "src": "iVBORw0KGgoAAAANSUhEUgAAAKsAAADVCAMAAAAfHv...",
  "type": "base64"
}
```

The response will then look like this:

```json
{
    "imageId": "vipps-socks-orange-123"
}
```

### Adding and changing Category

[`PUT:/v2/{paymentType}/categories/{orderId}`](https://vippsas.github.io/vipps-order-management-api/redoc.html#operation/putCategoryV2)

This is the endpoint used for adding and updating the Category for a Vipps Transaction. The category is mutable, and a new request will completely overwrite previous requests. Here is an example request:

```json
{
"category": "GENERAL",
"orderDetailsUrl": "https://www.vipps.no",
"imageId": "vipps-socks-orange-123"
}
```

## Receipts


By using the [`POST:/receipts`](https://vippsas.github.io/vipps-order-management-api/#/receipts/postReceipt) endpoint, it is possible to add a `Receipt` to a Vipps transaction. This is done by sending each OrderLine with its relevant VAT info. The Sum of the receipt will be calculated based on the orderlines that are sent in. In environments where a paper printer isn't accessible, this can be very valuable. The receipt generated in the Vipps app is an "electronic copy" and should be approved by all Norwegian accounting firms.

!["Extended order lines"](images/order-lines-extended_sm.png)

### Adding a Receipt

[`POST:/v2/{paymentType}/receipts/{orderId}`](https://vippsas.github.io/vipps-order-management-api/redoc.html#operation/postReceiptV2)

This is the endpoint for sending receipt information. Receipt information is a combination of a list of orderLines, and a bottom line with sum and vat. An OrderLine is a description of each item present in the order. Detailed information about each property is available in the [swagger](https://vippsas.github.io/vipps-order-management-api/redoc.html#operation/postReceiptV2). A receipt is immutable and, once sent, cannot be overwritten.

Example request:

Headers:

```text
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <snip>
Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a
Merchant-Serial-Number: 123456
Vipps-System-Name: Acme Enterprises Ecommerce DeLuxe
Vipps-System-Version: 3.1.2
Vipps-System-Plugin-Name: Point Of Sale Excellence
Vipps-System-Plugin-Version 4.5.6
```

Body:

```json
{
  "orderLines": [
    {
      "name": "Vipps socks",
      "id": "line_item_1",
      "totalAmount": 1000,
      "totalAmountExcludingTax": 800,
      "totalTaxAmount": 200,
      "taxPercentage": 25,
      "unitInfo": {
        "unitPrice": 400,
        "quantity": "2.5",
        "quantityUnit": "KG"
      },
      "discount": 0,
      "productUrl": "https://www.vipps.no/store/socks",
      "isRefund": false,
      "isShipping": false
    },
    {
      "name": "Vipps flip-flops",
      "id": "line_item_2",
      "totalAmount": 5000,
      "totalAmountExcludingTax": 4000,
      "totalTaxAmount": 1000,
      "taxPercentage": 25,
      "unitInfo": {
        "unitPrice": 2500,
        "quantity": "3",
        "quantityUnit": "PCS"
      },
      "discount": 2500,
      "productUrl": "https://www.vipps.no/store/flipflops",
      "isRefund": false,
      "isShipping": false
    },
        {
      "name": "Home delivery",
      "id": "shipping_1",
      "totalAmount": 1000,
      "totalAmountExcludingTax": 1000,
      "totalTaxAmount": 0,
      "taxPercentage": 0,
      "discount": 0,
      "isRefund": false,
      "isShipping": true
    }
  ],
  "bottomLine": {
    "currency": "NOK",
    "tipAmount": 0,
    "giftCardAmount": 0,
    "terminalId": "vipps_pos_122"
  }
}
```

### Fetching Category and Receipt

[`GET:/v2/{paymentType}/{orderId}`](https://vippsas.github.io/vipps-order-management-api/redoc.html#operation/getOrderV2)

This endpoint is used for getting both `Category` and `Receipt` for the Vipps Transaction. The response body includes ID references to images previously uploaded, Links, and the OrderLines added.

## Vipps Assisted Content Monitoring

Vipps offers assisted content monitoring as a way for Merchants to deal with the regulatory demands of content monitoring. For some merchants Vipps can utilize the merchant's webpage for content monitoring, continuously verifying that the actual products being sold coincides with the expected products.

### Mandatory use case

If you, as a merchant, do not have a permanent website that can be utilized for content monitoring, for example you do not have a user facing website or the website is ephemeral/short lived then you must utilize Vipps Assisted Content Monitoring.

In order to comply with Vipps Assisted Content Monitoring all transactions must be posted to the Order Management receipts functionality described in the [Receipts](#receipts) section.

## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-ecom-api/issues),
a [pull request](https://github.com/vippsas/vipps-ecom-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).
