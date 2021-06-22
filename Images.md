# Images
| Operation | Description | Endpoint |
| --------- | ----------- | -------- |
| [Upload image](#cancel) | Upload an image| `POST:/order-management/v1/image` |

The api experimentally supports image uploading. An image is uploaded in base64 format to the image endpoint and can then be used on any number of orders.

For now supported image formats are jpg and png.

|Parameter | Type | Required | Description|
|----------|------|----------|------------|
| `imageId` | `string` | Y | Unique identifier of image|
| `src` | `string` | Y | base64 encoded image|
| `type` | `string` | Y | Type of image. Only base64 is supported right now.|

Example request of uploading one image

## Upload image
	PUT:/order-management/v1/image
Example request body
```
{
  "imageId": "image1234",
  "src": "dGhpcyBpcyBhbiBpbWFnZQ==",
  "type": "base64"
}
```
Remarks  
imageId: Only A-Z, a-z, 0-9, '-', '_', '.' are valid characters

[Getting started](GettingStarted.md)  
[Orders](Orders.md)  
[Images](Images.md)  
[Receipts](Receipts.md)
