# Orders
| Operation | Description | Endpoint |
| --------- | ----------- | -------- |
| [Update order](#update-order)| Update order with enriched data | `PUT:/order-management/v1/order/{transactionId}` |
| [Get order](#get-order) | Fetch the existing data for an order | `GET:/order-management/v1/order/{transactionId}`   |

|Parameter | Type | Required | Description|
|----------|------|----------|------------|
| `category` | `string` | Y | general, receipt, orderConfirmation, delivery, ticket or booking|
| `orderDetailsUrl` | `string` | Y | Must be secure, absolute and valid url or app-native url |
| `image` | `imageId` | N |   |
| `imageId.id` | `string` | Y | Only A-Z, a-z, 0-9, '-', '_', '.' are valid characters |


## Update order
	PUT:/order-management/v1/order/{transactionId}
Example request body
```
{
  "category": "general",
  "orderDetailsUrl": "https://mystore.no/order/987654",
  "image": 
    {
      "id": "img1234",
    }
}
Or  
{
  "category": "ticket",
  "orderDetailsUrl": "ruter:///min/bilett",
  "image": 
    {
      "id": "ruter_bilett",
    }
}
```
## Get order
	GET:/order-management/v1/order/{transactionId}
Example response 
```
{
  "category": "general",
  "orderDetailsUrl": "https://mystore.no/order/987654",
  "image": 
    {
      "id": "img1234",
    }
}
```

[Getting started](GettingStarted.md)  
[Images](Images.md)  
[Receipts](Receipts.md)
