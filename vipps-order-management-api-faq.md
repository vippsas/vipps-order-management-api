---
title: Order Management API Frequently Asked Questions
sidebar_label: FAQ
sidebar_position: 60
description: Frequently asked questions for the Order Management API.
pagination_next: null
pagination_prev: null
---

# Frequently asked questions

Here are the Order Management API FAQs.
See the
[Order Management API guide](vipps-order-management-api.md)
for all the details.

For general information and questions, please check in the
[Knowledge base](https://developer.vippsmobilepay.com/docs/common-topics/).

## Can I combine different `category` types?

No, a payment can only have one `category`.
When you do a
[`PUT/order-management/v2/{paymentType}/categories/{orderId}`](https://developer.vippsmobilepay.com/api/order-management/#tag/Category/operation/putCategoryV2)
it will replace an existing `category`.

## Can I update `orderLines` for a payment?

No, not yet. It is only possible to do
[`POST:/order-management/v2/{paymentType}/receipts/{orderId}](https://developer.vippsmobilepay.com/api/order-management/#tag/Receipt/operation/postReceiptV2)
once.

This is a known limitation, and we will update here if this changes.

