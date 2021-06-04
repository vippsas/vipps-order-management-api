# Images

The api experimentally supports image uploading. An image is uploaded in base64 format to the image endpoint and can then be used on any number of orders.

For now supported image formats are jpg and png.

Parameter | Type | Required | Description
----------|------|----------|------------
`imageId` | `string` | Y | Unique identifier of image
`src` | `string` | Y | base64 encoded image
`type` | `string` | Y | Type of image. Only base64 is supported right now.

Example request of uploading one image