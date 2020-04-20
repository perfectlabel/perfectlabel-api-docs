
# Perfect Label and Postman
This document descirbes how to try the API using Postman

Before starting working with the API, it's required to contact Perfect Label support team and get an authentication token (please email support@perfectlabel.io to request it).

## Setting Up Postman

1. Please make sure Postman installed https://www.postman.com/
2. Import postman collection.
2.1. Click the "Import" button in the top left corner. (Screenshot: https://share.getcloudapp.com/p9uKb5XN)
2.2. Click the "Choose Files" button, select the "perfectlabel.postman_collection.json" file from the postman folder and click the "Open" button. (Screenshot: https://share.getcloudapp.com/OAurPL68)
3. Add a new environment:
3.1. Click a wheel button in the top right corner
3.2. Click "Import" button in the "Manage Environments" window (Screenshot: https://share.getcloudapp.com/Z4u5ow2Z)
3.3. Click the "Choose Files" button (Screenshot: https://share.getcloudapp.com/jkuKLnpg), select the "perfectlabel.postman_environment.json" file from the postman folder and click the "Open" button (Screenshot: https://share.getcloudapp.com/2NuBwrQ6)
3.4. Set your AUTH_TOKEN
3.5. Close the "Manage Environments" window

### Check Installation
1. Select the "Check that the token is correct" request
2. Click the "Send" button


## Simple Unwrap API

Simple unwrap API takes an image as "img_original" parameter, and returns it unwrapped.
