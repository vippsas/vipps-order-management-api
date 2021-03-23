# Vipps order management api v1

Api for adding and retreiving info about vipps transactions. The added data will be shown on the receipt in the vipps app. This works for both recurring and direct transactions, but not for passthrough payments.
In this setup, images are handled detatched from transactions. This means that the merchant could upload one image and reuse that for all orders, and wont need to send the same image for every transaction. Images needs to be added before the metadata for a transaction is added.

Api breakdown:
- Merchant calls /image to upload an image to vipps by adding a b64 string in the body.
	- The merchant can add as many images as they want, and they are not linked to any transactions (yet)
    - merchant saves the imageId
- Merchant does a post to /order/{{vippsTransactionId} with following data:
	- category: Defining what kind of purchase this is (e.g. receipt or ticket++)
	- orderDetailsUrl: Link back to merchant page
	- imageIds: list of the already uploaded images to be linked to the payment, using imageId

