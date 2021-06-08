# Vipps order management api v1

The Order Management API allows merchants to send rich receipt information to existing Vipps transaction. This information is shown to the customer in the app in their order history.
This functionality is available for both recurring and direct payments, but not for passthrough payments.

In this setup, merchants are able to send images, receipts (order lines) and other information. Images are handled detached from transactions. This means that the merchant could upload one image and reuse it for several orders. Images must be added before the metadata for a transaction.

# Api summary:
- `/image`
	- Endpoint for uploading pictures of products as a b64 string, along with an ID defined by the merchant. An image exists independently of any transaction. It is not possible to overwrite an image.
- `/receipt/{vippsTransactionId}`
	- Endpoint for sending receipt information (an array of order lines). Order lines are descriptions of each item present in an order. A receipt is immutable and, once sent, cannot be overwritten.
- `/order-management/v1/order/{vippsTransactionId}`
	- Endpoint for sending all other information about a transaction, including ID references to images previously uploaded. This object is mutable, and a new request will completely overwrite previous requests.


[Getting started](GettingStarted.md)  
[Images](Images.md)  
[Receipts](Receipts.md)
## API endpoints

| Operation | Description | Endpoint |
| -------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| [Update order](#initiate)| Update order with enriched data | `PUT:/order-management/v1/order/{transactionId}` |
| [Get order](#capture) | Fetch the existing data for an order | `GET:/order-management/v1/order/{transactionId}`   |
| [Upload image](#cancel) | Upload an image| `POST:/order-management/v1/image` |
| [Upload receipt](#refund) | Upload a receipt                                                                | `POST:/order-management/v1/receipt/{transactionId}` | [Update Receipt]()




## Update order
	PUT:/order-management/v1/order/{transactionId}
Example request body
```
{
  "Category": 1,
  "OrderDetailsUrl": "https://mystore.no/order/987654",
  "ImageIds": [
    "img1234",
	"img5679"
  ]
}
```
## Upload image
	PUT:/order-management/v1/image
Example request body
```
{
  "ImageId": "image1234",
  "Src": "dGhpcyBpcyBhbiBpbWFnZQ==",
  "Type": 1
}
```
## Receipt
	PUT:/order-management/v1/receipt/{transactionId}
Price amounts are given in the lowest currency unit (usually cents).

Example request body
```
{
  "OrderLines": [
    {
      "AmountExcludingTax": 10000,
      "AmountIncludingTax": 12000,
      "Description": "cotton t-shirt (blue, medium)",
      "Id": "998877",
      "ProductUrl": "https://mystore.no/products/998877",
      "Quantity": 1,
      "TaxAmount": 2000,
      "TaxPercentage": 20
    }
  ]
}
```


