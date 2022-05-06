# Vipps Order Management API v1

The Order Management API allows merchants to enrich Vipps Transactions.
The information given in this API will be shown to the customer in the order
history in their app. This functionality is available for both recurring and
direct payments, but not for passthrough payments.

In this setup, merchants are able to send images, receipts (order lines) and
other information. Images are handled detached from transactions. This means
that the merchant could upload one image and reuse it for several orders.
Images must be added before the metadata for a transaction.

Vipps Order Management enables you to communicate with your customers through
the payment receipts in the Vipps app. The purpose of doing this is to give
your customers more convenience, better overview and a more compelling shopping
experience when they use Vipps to pay for your products and services.
Vipps Order management also enables you to draw customers back to your website
or app from links on the Vipps receipt view.

API version: 1.0.2.

Document version: 0.1.2.

## Table of contents

- [Vipps Order Management capabilities](#vipps-order-management-capabilities)
- [OrderInfo](#orderinfo)
  - [link](#link)
  - [Image](#image)
- [Receipt](#receipt)
- [Getting Started](#getting-started)
  - [Before you begin](#before-you-begin)
  - [Authentication](#authentication)
  - [Vipps HTTP headers](#vipps-http-headers)
    - [Example headers](#example-headers)
- [Vipps Assisted Content Monitoring](#vipps-assisted-content-monitoring)
  - [Mandatory use case](#mandatory-use-case)
  - [Guidance](#guidance)
- [API summary](#api-summary)
  - [Images](#images)
  - [Receipts](#receipts)
    - [Recommended integration (currently in pilot mode)](#recommended-integration-currently-in-pilot-mode)
    - [Legacy integration](#legacy-integration)
  - [OrderInfo](#orderinfo-1)
- [Questions?](#questions)

## Vipps Order Management capabilities

Vipps Order Management currently has the following capabilities:

* OrderInfo:
  * Image
  * Link
  * Category
* Receipt

`OrderInfo` is added using the [`PUT:/orders`](https://vippsas.github.io/vipps-order-management-api/#/orders/putOrder) endpoint. With this endpoint it is
possible to add an `Image`, a `Link` and a `Category` to a payment. The
[`POST:/receipts`](https://vippsas.github.io/vipps-order-management-api/#/receipts/postReceipt) endpoint is used to add receipt info like `orderLines` and VAT
information.

## OrderInfo

The [`PUT:/orders`](https://vippsas.github.io/vipps-order-management-api/#/orders/putOrder) endpoint allows the addition of `Image`, `Links` and `Categories`
to Vipps transactions. `Link` and `Category` is mandatory when using this API,
and `Images` are optional.

### Link

In order to provide customers with more up to date information about their order,
you can add a URL or link to the payment receipt in Vipps that can take the
customer to a location on your website. Links are activated when a customer
clicks the link area on the Vipps receipt. The mobile device's standard web
browser will open and the user is redirected to the link location.

Below you can se an example of a Vipps receipt containing a link to "Shipping
information".
<p align="center">
  <img src="images/order-link-shipping-information.png" alt="Shipping information link" width="250">
</p>
OrderInfo has a category that will affect infographics and how it is handled
in the Vipps app. We currently support these categories:

* **Receipt**: A link to a location where the customer can access and download
  a valid proof of purchase and receipt for this particular order
* **Order Confirmation**: A link to a location that contains information and
  status of the order. If your webshop or site has a "My orders" page or
  similar - this link category can take the customer there.
* **Delivery information**: A link to a location that contains information and
  status about the shipping or delivery related to the order. This could be a
  link to hosted by your freight carrier, or a link to your site. If your
  webshop or site has a "My order" page that includes delivery related
  information about the order - this link category can be used.
* **Ticket**: A link to a location where the customer can access and download a
  ticket to an event, trip or transportation.
* **Booking**: A link to a location that contains information and status about a
  booking, such as travel and rental booking. If your webshop or site has a
  "My bookings" page or similar, this link category can take the customer there.
* **General**: If none of the other categories fit the use case for the link,
  a *General* category can be used. This is a link to a location that contains
  any kind of information pertinent to the order. We encourage you to use the
  more specific categories if possible.

### Image

With Vipps Order Management API you can upload an image that is shown on the
transaction in the Vipps app. To add an image to an order you first need to use
the [`POST:/images`](https://vippsas.github.io/vipps-order-management-api/#/images/postImage) endpoint to upload an image. Thereafter, the newly uploaded
image's `imageId` can be added using the `orderInfo` API. The same image may
be used for multiple transactions, but uploading a unique image for each
transactions is also OK. Imaged are fetched authenticated from the app, so feel
free add tickets and receipts as images. Below you can see an example of a Vipps
receipt containing an image of the shopping card (single product)
<p align="center">
  <img src="images/order-link-shipping-information-with-image.png"
       alt="Shipping information link" width="150">
</p>

## Receipt

In addition to providing a user with a link to a valid receipt hosted on your
site, it is also possible have the receipt hosted inside the Vipps app. To
enable this you can send necessary information such as order lines and VAT rates
to the Order Management API and Vipps will generate a receipt that can be used
for proof of purchase and expensing. This is done using the [`POST:/receipts`](https://vippsas.github.io/vipps-order-management-api/#/receipts/postReceipt) endpoint.

In addition to providing the user with a valid receipt inside Vipps, order lines
will also give the user a much better overview of the purchase. Furthermore, in
the case of partial order fulfilments or returns the order lines may be updated
to reflect what the user actually ended up paying for using Vipps.

<p align="center">
  <img src="images/orderlines.png" width="150">
</p>

## Getting Started

### Before you begin

This section covers the quick steps for getting started with the Order Management
API to enrich orders with metadata. This document assumes you have signed up as
a organisation with Vipps and have your test credentials from the Merchant Portal.

It also assumes a payment has been initialized and reserved and that you have
the transaction id.

### Authentication

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

#### Example headers

If the vendor's name is "Acme AS", and the vendor offers two different systems
one for point of sale (POS) integrations and one for web shops,
the headers should be:

| Header                        | Example value for POS | Example value for webshop | Example value for Vending machines |
| ----------------------------- | --------------------- | ------------------- | ------------------- |
| `Merchant-Serial-Number`      | `123456`              | `123456`            | `123456`            |
| `Vipps-System-Name`           | `acme`                | `acme`              | `acme`              |
| `Vipps-System-Version`        | `1.7`                 | `2.6`               | `2.6`               |
| `Vipps-System-Plugin-Name`    | `acme-pos`            | `acme-webshop`      | `acme-vending`      |
| `Vipps-System-Plugin-Version` | `3.2`                 | `4.3`               | `4.3`               |

**Important:** Please use self-explanatory, human readable and reasonably short
values that uniquely identify the system (and plugin).

## Assisted

Vipps offers assisted content monitoring as a way for Merchants to deal with the regulatory demands of content monitoring. For some merchants Vipps can utilize the merchant's webpage for content monitoring, continuously verifying that the actual products being sold coincides with the expected products.

### Mandatory use case

However if you as a merchant does not have a permanent website that can be utilized for content monitoring, for example you do not have a user facing website or the website is ephemeral/short lived then you must utilize Vipps Assisted Content Monitoring.

### Guidance

In order to comply with Vipps Assisted Content Monitoring all transactions must be posted to the Order Management receipts functionality described in the [Receipts](#receipts) section.

## API summary

### Images

[`POST:/order-management/v1/images`](https://vippsas.github.io/vipps-order-management-api/#/images/postImage)

Endpoint for uploading pictures of products as a base64 string, along with an
ID defined by the merchant. An image exists independently of any transaction.
It is not possible to overwrite an image.

### Receipts

[`POST:/order-management/v1/receipts/{transactionId}`](https://vippsas.github.io/vipps-order-management-api/#/receipts/postReceipt)

[`GET:/order-management/v1/receipts/{transactionId}`](https://vippsas.github.io/vipps-order-management-api/#/receipts/getReceipt)

Endpoints for sending and receiving receipt information (an array of order lines).
Order lines are descriptions of each item present in an order.
A receipt is immutable and, once sent, cannot be overwritten.

#### Recommended integration (currently in pilot mode)

In order to integrate with the receipts functionality, you need to retrieve the `pspReference` in the `"paymentAction": "AUTHORISATION"` event from the [eventlog](https://vippsas.github.io/vipps-epayment-api/index.html#operation/getPaymentEventLog) endpoint. *This is required if utilizing receipts with Vipps Checkout or Free standing card payments* The `pspReference` must then be utilized as the transactionId in the [`POST:/order-management/v1/receipts/{transactionId}`](https://vippsas.github.io/vipps-order-management-api/#/receipts/postReceipt) endpoint.

#### Legacy integration

Optionally you may integrate with the [EcomV2Api](https://github.com/vippsas/vipps-ecom-api), retrieving the `transactionLogHistory.transactionId` value found in the [details](https://vippsas.github.io/vipps-ecom-api/#/Vipps%20eCom%20API/getPaymentDetailsUsingGET) endpoint. *This will not be compatible with Vipps Checkout or Free Standing Card Payments*.

### OrderInfo

[`PUT:/order-management/v1/orders/{transactionId}`](https://vippsas.github.io/vipps-order-management-api/#/orders/putOrder)

[`GET:/order-management/v1/orders/{transactionId}`](https://vippsas.github.io/vipps-order-management-api/#/orders/getOrder)

Endpoints for sending and retrieving OrderInfo for a transaction, including ID
references to images previously uploaded and Links for redirection back to merchants.
This object is mutable, and a new request will completely overwrite previous requests.

## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-ecom-api/issues),
a [pull request](https://github.com/vippsas/vipps-ecom-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).
