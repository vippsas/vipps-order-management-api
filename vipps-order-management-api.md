# Getting Started

## Before you begin
This document covers the quick steps for getting started with the Order Management API to enrich orders with metadata. This document assumes you have signed up as a organisation with Vipps and have your test credentials from the Merchant Portal.

It also assumes a payment has been initialized and reserved and that you have the transaction id.

## 1. Authentication
```bash
curl https://apitest.vipps.no/accessToken/get \
-H "client_id: YOUR-CLIENT-ID" \
-H "client_secret: YOUR-CLIENT-SECRET" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-X POST
```

In response you will get a body with the following schema.
The property `access_token` should be used for all other API requests in the `Authorization` header as the Bearer token.

The access token is obtained as described in
[Getting started](https://github.com/vippsas/vipps-developers/blob/master/vipps-getting-started.md):
[Get an access token](https://github.com/vippsas/vipps-developers/blob/master/vipps-getting-started.md#get-an-access-token) 

```json
{
  "token_type": "Bearer"
  },
  "expires_in": "3599"
  },
  "ext_expires_in": "3599"
  },
  "expires_on": "1614116654"
  },
  "not_before": "1614112754"
  },
  "resource": "00000002-0000-0000-c000-000000000000"
  },
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyIsImtpZCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyJ9.eyJhdWQiOiIwMDAwMDAwMi0wMDAwLTAwMDAtYzAwMC0wMDAwMDAwMDAwMDAiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9lNTExNjUyNi01MWRjLTRjMTQtYjA4Ni1hNWNiNDcxNmJjNGIvIiwiaWF0IjoxNjE0MTEyNzU0LCJuYmYiOjE2MTQxMTI3NTQsImV4cCI6MTYxNDExNjY1NCwiYWlvIjoiRTJaZ1lMQmF1V25qcG12c2NhYlhJOTliSmt3c0FRQT0iLCJhcHBpZCI6IjIyNWVmMTU5LWFjZjAtNGRiNy04OGU0LWNlNDMyODYxOWM3MyIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0L2U1MTE2NTI2LTUxZGMtNGMxNC1iMDg2LWE1Y2I0NzE2YmM0Yi8iLCJyaCI6IjAuQVNBQUptVVI1ZHhSRkV5d2hxWExSeGE4UzFueFhpTHdyTGROaU9UT1F5aGhuSE1nQUFBLiIsInRlbmFudF9yZWdpb25fc2NvcGUiOiJFVSIsInRpZCI6ImU1MTE2NTI2LTUxZGMtNGMxNC1iMDg2LWE1Y2I0NzE2YmM0YiIsInV0aSI6Imo4bHRFZER5R2tDeURtdXR3QVVNQUEiLCJ2ZXIiOiIxLjAifQ.IeLADJiz5WRdQgf-3LfnUCfiQpKNjRIJjvDfYzoG9xgOQwBhSKeDlelIx0_FMx3oHtvYkGWebDy0Y1HjdrbgzoA2RTeIzS8IjylZcGfSuhA6kUvBa4JUPLW4Irefp3Bv77gUfS0dVzHVILADV-8VSCjivld7ovEANQagupsi4zhAyVWuNuHurDOSSI33lxnes-FmphUfiUfwmye9B676lwaj28I1dP3JxqFDDf3SNkjNLvTZyiDaIprZrt4TC_t5eopzqCL4X1ymnWxzJzMMPQGVOvhNEJj1oI_5VbRtoYdo_b5bYsU5ZS7JSGcuOpog7vEtVk6uJDDT0MfQIuOLaeA"
  }
}
```
## 2. Enrich order

Parameter | Type | Required | Description
----------|------|----------|------------
`category` | `string` | Y | Category of order
`orderDetailsUrl` | `string` | Y | Url to external page with order details
`imageIds` | `string[]` | N | Images associated with this order  


To add meta data to an order send a request:   

```bash
curl https://apitest.vipps.no/order-management/v1/order/{transactionId} \
-H "Authorization: Bearer <TOKEN>" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-H "Content-Type: application/json" \
-X PUT
-d '{
    "category": "general",
    "orderDetailsUrl": "https://yourwebsite.come/orderDetails?orderId=abcc123",
    "imageIds" : []
}'
```

A valid request like the one above will result in a response with the following structure. **(TODO check response)**

```json
{
  "id": "id of the transaction"
}
```
