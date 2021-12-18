# Match Products APIs
## Initial Setup
Contact support@perfectlabel.io to get a new matching project created, and 
retrieve the following information:
* Project id
* Access token with admin permissions (required to add/delete products to the project)
* Client access token (required to access a public product search API)

## Manage Products
For reverse image search among products, it's required to add their front images
to the current project first.

### Add Products 

#### Pass base64-encoded image
* Encode content of JPEG image as base64, and pass it as `img_original_base64` parameter
* Pass `data` parameter with JSON, containing the information you want to get returned, if the product
was found by the search API. Usually it's the ID of the product in your database.
```bash
$ export AUTH_TOKEN=<YOUR_TOKEN_HERE>
$ export PROJECT_ID=<YOUR_PROJECT_ID>
$ curl -H "Authorization: Token ${AUTH_TOKEN}" https://perfectlabel.io/api/v001/projects/${PROJECT_ID}/products/ \
  -XPOST -H "Content-Type: application/json" \
    -d '{"img_original_base64": "PHJlcGxhY2Ugd2l0aCBjb250ZW50IG9mIGpwZWcgZmlsZT4=", "data": {"my_product_id": 123, "product_name": "Koval Winery Cabernet Sauvignon 2018"}'
```
Response:
```json
{
    "id": 75,
    "created": "2021-12-18T01:18:39.861166Z",
    "updated": "2021-12-18T01:18:39.866024Z",
    "status": "pending",
    "data": {
        "my_product_id": 123,
        "product_name": "Koval Winery Cabernet Sauvignon 2018"
    },
    "img_original": {
      "thumb_url": "",
      "created": "2021-12-18T01:18:39.837331Z",
      "updated": "2021-12-18T01:18:39.851924Z",
      "full_url": "https://perfect-label-uploads.s3.amazonaws.com/images/78d75b31-40f0-406f-8283-2afc065fbbd1.jpg",
      "id": 661
    }
}
```

The api will return a created project

### List Products
```bash
$ export AUTH_TOKEN=<YOUR_TOKEN_HERE>
$ export PROJECT_ID=<YOUR_PROJECT_ID>
$ curl -H "Authorization: Token ${AUTH_TOKEN}" https://perfectlabel.io/api/v001/projects/${PROJECT_ID}/products/?product_fields=id,status,data
```

### Filter Products by Status

Product statuses are:
* `pending` - a product is added to the system, but not processed by the pipelines (i.e. not searchable) 
* `complete` - a product is searchable
* `error` - failed to process a product (for example, the uploaded image was corrupted)

Sometimes it's required to fetch products of a specific status, this request is similar to list API,
but an additional parameter "status" is passed:

```bash
$ export AUTH_TOKEN=<YOUR_TOKEN_HERE>
$ export PROJECT_ID=<YOUR_PROJECT_ID>
$ curl -H "Authorization: Token ${AUTH_TOKEN}" https://perfectlabel.io/api/v001/projects/${PROJECT_ID}/products/?status=complete&product_fields=id,status,data
```

### Get Product by ID

```bash
$ export AUTH_TOKEN=<YOUR_TOKEN_HERE>
$ export PROJECT_ID=<YOUR_PROJECT_ID>
$ export PRODUCT_ID=<PRODUCT_ID>
$ curl -H "Authorization: Token ${AUTH_TOKEN}" https://perfectlabel.io/api/v001/projects/${PROJECT_ID}/products/${PRODUCT_ID}/?product_fields=id,status,data
```


### Delete Product
```bash
$ export AUTH_TOKEN=<YOUR_TOKEN_HERE>
$ export PROJECT_ID=<YOUR_PROJECT_ID>
$ export PRODUCT_ID=<PRODUCT_ID>
$ curl -H "Authorization: Token ${AUTH_TOKEN}" -XDELETE https://perfectlabel.io/api/v001/projects/${PROJECT_ID}/products/${PRODUCT_ID}/
```
