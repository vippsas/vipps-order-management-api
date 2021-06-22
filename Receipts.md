# Receipts
| Operation | Description | Endpoint |
| --------- | ----------- | -------- |
| [Upload receipt](#refund) | Upload a receipt  | `POST:/order-management/v1/receipt/{transactionId}` |
| [Get receipt]() |  | | `GET:/order-management/v1/receipt/{transactionId}` |



The api supports receipt uploading. 


|Parameter | Type | Required | Description|
----------|------|----------|------------
|`amountExcludingTax` | `integer` | Y | Currency without delimiter eg NOK 10.25 is 1025 |


Example request of uploading one image



## Receipt
	PUT:/order-management/v1/receipt/{transactionId}
Price amounts are given in the lowest currency unit (usually cents).

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
      "taxPercentage": 20
    }
  ]
}
```

[Getting started](GettingStarted.md)  
[Orders](Orders.md)  
[Images](Images.md)  
