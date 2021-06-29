# Receipts
| Operation | Description | Endpoint |
| --------- | ----------- | -------- |
| [Upload receipt](#upload-receipt) | Upload a receipt  | `POST:/order-management/v1/receipt/{transactionId}` |
| [Get receipt](#get-receipt) | Get an uploaded receipt | `GET:/order-management/v1/receipt/{transactionId}` |



The api supports receipt uploading. 
### Receipt
|Parameter | Type | Required | Description|
----------|------|----------|------------
|[`orderLines`](#orderLine) | `array of OrderLine` | Y | Minimum one element |
|`shippingAmount` | `integer` | N | Currency without delimiter eg NOK 100.00 is 10000 |

### OrderLine 
|Parameter | Type | Required | Description|
----------|------|----------|------------
|`amountExcludingTax` | `integer` | Y | Currency without delimiter eg NOK 10.25 is 1025 |
|`amountIncludingTax` | `integer` | Y | Currency without delimiter eg NOK 10.25 is 1025 |
|`description` | `string` | Y | Item description |
|`id` | `string` | Y | Item identifier |
|`productUrl` | `string` | N | Valid secure url or app native url |
|`quantity` | `integer` | Y | Number of purchased items |
|`taxAmount` | `integer` | Y | Currency without delimiter eg NOK 10.25 is 1025 |
|`taxPercentage` | `integer` | Y | Number without delimiter eg 15% is 1500 |
|`discount` | `integer` | Y | Currency without delimiter eg NOK 10.25 is 1025 |


## Upload receipt
	PUT:/order-management/v1/receipt/{transactionId}

Example request body
```
{
  "orderLines": [
    {
      "amountExcludingTax": 10000,
      "amountIncludingTax": 12000,
      "description": "cotton t-shirt (blue, medium)",
      "id": "998877",
      "productUrl": "https://mystore.no/products/998877",
      "quantity": 1,
      "taxAmount": 2000,
      "taxPercentage": 2000,
      "discount": 0
    }
  ],
  "shippingAmount": 15000
}
```

## Get receipt
	GET:/order-management/v1/receipt/{transactionId}

Example response body
```
{
  "orderLines": [
    {
      "amountExcludingTax": 10000,
      "amountIncludingTax": 12000,
      "description": "cotton t-shirt (blue, medium)",
      "id": "998877",
      "productUrl": "https://mystore.no/products/998877",
      "quantity": 1,
      "taxAmount": 2000,
      "taxPercentage": 2000,
      "discount": 0
    }
  ],
  "shippingAmount": 15000
}
```



[Getting started](GettingStarted.md)  
[Orders](Orders.md)  
[Images](Images.md)  
