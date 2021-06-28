# Vipps order management api v1

The Order Management API allows merchants to send rich receipt information to existing Vipps transaction. This information is shown to the customer in the app in their order history.
This functionality is available for both recurring and direct payments, but not for passthrough payments.

In this setup, merchants are able to send images, receipts (order lines) and other information. Images are handled detached from transactions. This means that the merchant could upload one image and reuse it for several orders. Images must be added before the metadata for a transaction.

# Api summary:
- `/order-management/v1/image`
	- Endpoint for uploading pictures of products as a b64-string, along with an ID defined by the merchant. An image exists independently of any transaction. It is not possible to overwrite an image.
- `/order-management/v1/receipt/{vippsTransactionId}`
	- Endpoint for sending receipt information (an array of order lines). Order lines are descriptions of each item present in an order. A receipt is immutable and, once sent, cannot be overwritten.
- `/order-management/v1/order/{vippsTransactionId}`
	- Endpoint for sending all other information about a transaction, including ID references to images previously uploaded. This object is mutable, and a new request will completely overwrite previous requests.


[Getting started](GettingStarted.md)  

## API endpoints
[Orders](Orders.md)  
[Images](Images.md)  
[Receipts](Receipts.md)

