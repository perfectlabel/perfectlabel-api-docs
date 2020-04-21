
# Perfect Label and Postman
This document descirbes how to try the API using Postman

Before starting to work with the API, it's required to contact Perfect Label support team and get an authentication token (please email support@perfectlabel.io to request it).

## Setting Up Postman

* Please make sure Postman installed https://www.postman.com/

### Import Postman Collection
* Click the "Import" button in the top left corner. ![Screenshot](/screenshots/postman-1.png)
* Click the "Choose Files" button, select the "perfectlabel.postman_collection.json" file from the postman folder and click the "Open" button.

### Setup an Environment
* Click a wheel button in the top right corner
* Click "Import" button in the "Manage Environments" window ![Screenshot](/screenshots/postman-2.png)
* Click the "Choose Files" button, select the "perfectlabel.postman_environment.json" file from the postman folder and click the "Open" button
* Click the newly imported environment ![Screenshot](/screenshots/postman-3.png)
* Set your AUTH_TOKEN value ![Screenshot](/screenshots/postman-4.png)
* Click UPDATE button and close the "Manage Environments" window
* Select "perfectlabel environment" in the newly created project ![Screenshot](/screenshots/postman-5.png)

### Check Installation
* Select the "Check that the token is correct" request
* Click the "Send" button

## API Usage

### Simple unwrap API
Simple unwrap API takes an image as "img_original" parameter, and returns it unwrapped.
That API is simple to use, and is useful, when it's required only to unwrap labels
(without stitching).
1. Open "Simple unwrap API" request in Postman
2. Select file to unwrap
3. Click "Send"
4. Find "img_unwrapped.full_url" attribute of the response

![Screenshot](/screenshots/postman-6.png)

### Unwrap and stitch

For image stitching, it's a bit more complicated.

1. Create a new project (it can be reused for all future labels)
2. Create a new label within the project
3. Upload label fragments
4. Execute "stitch" request

Please go through the related methods in Postman project, and execute all steps.
Please make sure to select the correct files, when uploading label fragments, and
keep the in proper order (from left to the right), otherwise stitching will fail
with error.
There are sample images provided in "postman/samples" directory.
