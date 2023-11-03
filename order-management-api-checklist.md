<!-- START_METADATA
---
title: Order Management API checklist
sidebar_label: Checklist
sidebar_position: 50
description: Checklist for full integration with the Order Management API.
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Checklist

API version: 1.0.

## Endpoints to integrate

Integrate _all_ the [API endpoints][order-mgmt-api-reference-url]. For examples of requests and responses, see the [Postman collection](/tools/order-management-api-postman-collection.json) and [environment](https://github.com/vippsas/vipps-developers/blob/master/tools/global-postman-environment.json).

| Endpoint | Comment |
|----------|---------|
| Add an image to an order | [`POST:/order-management/v1/images`][add-image-endpoint] |
| Add category to an order | [`PUT:/order-management/v2/{paymentType}/categories/{orderId}`][add-category-endpoint] |
| Add receipt to an order | [`POST:/order-management/v2/{paymentType}/receipts/{orderId}`][add-receipt-endpoint] |
| Get order with category and receipt | [`GET:/order-management/v2/{paymentType}/{orderId}`][get-order-endpoint] |

As this API allows you to update transactions that have been created by using the [eCom API](https://developer.vippsmobilepay.com/docs/APIs/ecom-api), [ePayment API](https://developer.vippsmobilepay.com/docs/APIs/epayment-api), or [Recurring API](https://developer.vippsmobilepay.com/docs/APIs/recurring-api), you may need to implement the associated checklist(s) from these.

## Quality assurance

| Action | Comment |
|--------|---------|
|     Handle errors | Make sure to log and handle all errors. All integrations should display errors in a way that the users (customers and merchant employees/administrators) can see and understand them.|
|     Include standard HTTP headers | Send the [HTTP headers](https://developer.vippsmobilepay.com/docs/knowledge-base/http-headers) in all API requests for better tracking and troubleshooting (mandatory for partners and platforms, who must send these headers as part of the checklist approval). |

## Avoid integration pitfalls

| Action | Comment |
|--------|---------|
|     Follow design guidelines| The Vipps MobilePay branding must be according to the [design guidelines](https://developer.vippsmobilepay.com/docs/design-guidelines).|
|     Educate customer support| Make sure your customer service, etc. has all the tools and information they need available in _your_ system, through the APIs listed in the first item in this checklist, and that they do not need to visit [portal.vipps.no](https://portal.vipps.no) for normal work.|

## Flow to go live for direct integrations

1. The merchant orders
   [*Vipps på Nett*](https://www.vipps.no/produkter-og-tjenester/bedrift/ta-betalt-paa-nett/ta-betalt-paa-nett/).
1. Vipps MobilePay completes customer control (KYC, PEP, AML, etc.).
1. The merchant receives an email saying that they can log in with
   BankID on
   [portal.vipps.no][portal-url]
   and retrieve API keys.
1. The merchant completes all checklist items above.
   Please double-check to avoid mistakes.
1. The merchant verifies the integration in the production environment:
   * We recommend checking this using both the API itself and the API Dashboard available under _Utvikler_ on
      [portal.vipps.no][portal-url].
   * **Please note:** We don't do any kind of activation or make any changes based on this checklist.
      The API keys for the production environment are made available on
      [portal.vipps.no][portal-url]
      as soon as the customer control (see step 2) is completed, independently of this checklist.
1. The merchant goes live 🎉

## Flow to go live for direct integrations for partners

See: [partners](https://developer.vippsmobilepay.com/docs/partner).

[order-mgmt-api-reference-url]: https://developer.vippsmobilepay.com/api/order-management
[add-image-endpoint]: https://developer.vippsmobilepay.com/api/order-management#tag/Image/operation/postImage
[add-category-endpoint]: https://developer.vippsmobilepay.com/api/order-management#tag/Category/operation/putCategoryV2
[add-receipt-endpoint]: https://developer.vippsmobilepay.com/api/order-management#tag/Receipt/operation/postReceiptV2
[get-order-endpoint]: https://developer.vippsmobilepay.com/api/order-management#tag/Order/operation/getOrderV2
[portal-url]: https://portal.vipps.no
