# Orders
| Operation | Description | Endpoint |
| --------- | ----------- | -------- |
| [Update order](#initiate)| Update order with enriched data | `PUT:/order-management/v1/order/{transactionId}` |
| [Get order](#capture) | Fetch the existing data for an order | `GET:/order-management/v1/order/{transactionId}`   |

|Parameter | Type | Required | Description|
|----------|------|----------|------------|
| `category` | `string` | Y | general, receipt, orderConfirmation, delivery, ticket or booking|
| `orderDetailsUrl` | `string` | Y | Must be secure, absolute and valid url |
| `imageIds` | `array of imageId` | N | Maxs 4 images  |
| `imageId.id` | `string` | Y | Only A-Z, a-z, 0-9, '-', '_', '.' are valid characters |
| `imageId.sortOrder` | `integer` | N |  |


## Update order
	PUT:/order-management/v1/order/{transactionId}
Example request body
```
{
  "category": general,
  "orderDetailsUrl": "https://mystore.no/order/987654",
  "imageIds": [
    {
      "id": "img1234",
      "sortOrder": 1
    },
    {
      "id": "img5679",
      "sortOrder": 2
    }
  ]
}
```


[Getting started](GettingStarted.md)  
[Images](Images.md)  
[Receipts](Receipts.md)
