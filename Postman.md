
# Perfect Label and Postman
This document descirbes how to try the API using Postman

Before starting working with the API, it's required to contact Perfect Label support team and get an authentication token (please email support@perfectlabel.io to request it).

## Setting Up Postman

* Please make sure Postman installed https://www.postman.com/

### Import postman collection
* Click the "Import" button in the top left corner. ![Screenshot](/screenshots/postman-1.png)
* Click the "Choose Files" button, select the "perfectlabel.postman_collection.json" file from the postman folder and click the "Open" button.

Add a new environment:
* Click a wheel button in the top right corner
* Click "Import" button in the "Manage Environments" window !![Screenshot](/screenshots/postman-2.png)
* Click the "Choose Files" button, select the "perfectlabel.postman_environment.json" file from the postman folder and click the "Open" button
* Set your AUTH_TOKEN
* Close the "Manage Environments" window

### Check Installation
* Select the "Check that the token is correct" request
* Click the "Send" button


## Simple Unwrap API

Simple unwrap API takes an image as "img_original" parameter, and returns it unwrapped.
