# Vipps Order Management API v1

💥 DRAFT! This is unfinished work and subject to change. 💥

The Order Management API allows merchants to send rich receipt information to
existing Vipps transaction. This information is shown to the customer in the app in their order history.
This functionality is available for both recurring and direct payments, but not for passthrough payments.

In this setup, merchants are able to send images, receipts (order lines) and other information. Images are handled detached from transactions. This means that the merchant could upload one image and reuse it for several orders. Images must be added before the metadata for a transaction.

Vipps Order Management enables you to communicate with your customers through the payment receipts in the Vipps app. The purpose of doing this is to give your customers more convenience, better overview and a more compelling shopping experience when they use Vipps to pay for your products and services. Vipps Order management also enables you to draw customers back to your website or app from links on the Vipps receipt view.

# Table of contents

- [Vipps Order Management capabilities](#vipps-order-management-capabilities)
  * [Links](#links)
  * [Images of order (TBA)](#images-of-order--tba-)
  * [Order Lines and Proof of Purchase](#order-lines-and-proof-of-purchase)
- [Getting Started](#getting-started)
  * [Before you begin](#before-you-begin)
  * [1. Authentication](#1-authentication)
- [Api summary:](#api-summary-)
  * [API endpoints](#api-endpoints)
- [Orders](#orders)
  * [Update order](#update-order)
  * [Get order](#get-order)
- [Images (TBA)](#images--tba-)
  * [Upload image](#upload-image)
- [Receipts](#receipts)
    + [Receipt](#receipt)
    + [OrderLine](#orderline)
  * [Upload receipt](#upload-receipt)
  * [Get receipt](#get-receipt)
- [Questions?](#questions-)

# Vipps Order Management capabilities

Vipps Order Management currently has the following capabilities:

* Links
* Images of orders (TBA)
* Proof of purchase / Valid receipt

We expect to add even more capabilities in the future.

Images are handled detached from transactions. This means that the merchant could upload one image and reuse it for several orders. Images must be added before the metadata for a transaction.

## Links
In order to provide customers with more up to date information about their order, you can add a URL / link to the payment receipt in Vipps that can take the customer to a location on your website. Links are activated when a customer clicks the link area on the Vipps receipt. The mobile device's standard web browser will open and the user is redirected to the link location.

Below you can se an example of a Vipps receipt containing a link to "Shipping information".

![Shipping information link](images/order-link-shipping-information.png)

Links have a type or category that will affect infographics and how it is handled in the Vipps app. We currently support these cathegories:

- **Receipt**: A link to a location where the customer can access and download a valid proof of purchase and receipt for this particular order
- **Order Confirmation**: A link to a location that contains information and status of the order. If your webshop or site has a "My orders" page or similar - this link cathegory can take the customer there.
- **Delivery information**: A link to a location that contains information and status about the shipping or delivery related to the order. This could be a link to hosted by your freight carrier, or a link to your site. If your webshop or site has a "My order" page that includes delivery related information about the order - this link cathegory can be used.
- **Ticket**: A link to a location where the customer can access and download a ticket to an event, trip or transportation.
- **Booking**: A link to a location that contains information and status about a booking, such as travel and rental booking. If your webshop or site has a "My bookings" page or similar, this link cathegory can take the customer there.
- **General**: If none of the other cathegories fit the use case for the link, a *General* category can be used. This is a link to a location that contains any kind of information pertinent to the order. We encourage you to use the more specific cathegories if possible.

## Images of order (TBA)

With Vipps Order Management API you can upload an image that represents the order. This could be a specific image showing the product in this order, a collection of images that will be shown as a collage in Vipps. Below you can see an example of a Vipps receipt containing an image of the shopping card (single product)

![Shipping information link](images/order-link-shipping-information-with-image.png)

## Order Lines and Proof of Purchase

In addition to providing a user with a link to a valid receipt hosted on your site, it is also possible have the receipt hosted inside the Vipps app. To enable this you can send necessary information such as order lines and VAT rates to the Order Management API and Vipps will generate a receipt that can be used for proof of purchase and expensing.

In addition to providing the user with a valid receipt inside Vipps, order lines will also give the user a much better overview of the purchase. Furthermore, in the case of partial order fulfillments or returns the order lines may be updated to reflect what the user actually ended up paying for using Vipps.

![order lines example](images/orderlines.png)

# Getting Started

## Before you begin
This section covers the quick steps for getting started with the Order Management API to enrich orders with metadata. This document assumes you have signed up as a organisation with Vipps and have your test credentials from the Merchant Portal.

It also assumes a payment has been initialized and reserved and that you have the transaction id.

## Authentication

All Vipps API calls are authenticated and authorized with an access token
(JWT bearer token) and an API subscription key:

| Header Name                 | Header Value                | Description                                                                                                                                                                                                                                                                               |
| --------------------------- | --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Authorization`             | `Bearer <JWT access token>` | Type: Authorization token. This obtained as described in [Getting started](https://github.com/vippsas/vipps-developers/blob/master/vipps-getting-started.md): [Get an access token](https://github.com/vippsas/vipps-developers/blob/master/vipps-getting-started.md#get-an-access-token) |
| `Ocp-Apim-Subscription-Key` | Base 64 encoded string      | The subscription key for this API. This is available on [portal.vipps.no](https://portal.vipps.no).                                                                                                                                                                                       |

For more information about how to obtain an access token and all details around this, please see:
[Quick overview of how to make an API call](https://github.com/vippsas/vipps-developers/blob/master/vipps-getting-started.md#quick-overview-of-how-to-make-an-api-call)
in the
[Getting started guide](https://github.com/vippsas/vipps-developers/blob/master/vipps-getting-started.md).

## Optional Vipps HTTP headers

We recommend using the following _optional_ HTTP headers for all requests to the
Vipps eCom API. These headers provide useful metadata about the merchant's system,
which help Vipps improve our services, and also help in investigating problems.

| Header                        | Description                                  | Example value       |
| ----------------------------- | -------------------------------------------- | ------------------- |
| `Merchant-Serial-Number`      | The merchant serial number                   | `123456`            |
| `Vipps-System-Name`           | The name of the ecommerce solution           | `woocommerce`       |
| `Vipps-System-Version`        | The version number of the ecommerce solution | `5.4`               |
| `Vipps-System-Plugin-Name`    | The name of the ecommerce plugin             | `vipps-woocommerce` |
| `Vipps-System-Plugin-Version` | The version number of the ecommerce plugin   | `1.4.1`             |

These headers are required for plugins and partners and sent by the recent versions of
[the official Vipps plugins](https://github.com/vippsas/vipps-developers#plugins)
and we recommend all customers with direct integration with the API to also do so.

# API summary

See the API reference for details:
* [Swagger UI](https://vippsas.github.io/vipps-order-management-api/)
* [ReDoc](https://vippsas.github.io/vipps-order-management-api/redoc.html)

## Images

[`POST:/order-management/v1/images`](https://vippsas.github.io/vipps-order-management-api/#/images/postImage)

Endpoint for uploading pictures of products as a base64 string, along with an
ID defined by the merchant. An image exists independently of any transaction.
It is not possible to overwrite an image.

## Receipts

[`POST:/order-management/v1/receipts/{vippsTransactionId}`](https://vippsas.github.io/vipps-order-management-api/#/receipts/postReceipt)

[`GET:/order-management/v1/receipts/{vippsTransactionId}`](https://vippsas.github.io/vipps-order-management-api/#/receipts/getReceipt)

Endpoints for sending rand receiving receipt information (an array of order lines).
Order lines are descriptions of each item present in an order.
A receipt is immutable and, once sent, cannot be overwritten.

## Additional information

[`PUT:/order-management/v1/orders/{vippsTransactionId}`](https://vippsas.github.io/vipps-order-management-api/#/orders/putOrder)

[`GET:/order-management/v1/orders/{vippsTransactionId}`](https://vippsas.github.io/vipps-order-management-api/#/orders/getOrder)

Endpoints for sending and retrieving all other information about a transaction, including ID
references to images previously uploaded. This object is mutable, and a new
request will completely overwrite previous requests.

# Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-ecom-api/issues),
a [pull request](https://github.com/vippsas/vipps-ecom-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).
