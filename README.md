# Perfect Label REST API Docs
The repo describes [Perfect Label](https://perfectlabel.io) unwrapping/stitching REST API documentation.

Before starting working with the API, it's required to contact Perfect Label support team and get an authentication token (please email support@perfectlabel.io to request it).

To test authorization, use any endpoint (the sample below uses list projects endpoint) with the proper authorization header:
```bash
$ export AUTH_TOKEN=<YOUR_TOKEN_HERE>
$ curl -H "Authorization: Token ${AUTH_TOKEN}" https://perfectlabel.io/api/v001/projects/

```

```javascript
{"count":0,"next":null,"previous":null,"results":[]}
```

## Simple Unwrap API

Simple unwrap API takes an image as "img_original" parameter, and returns it unwrapped.


```bash
$ curl -H "Authorization: Token ${AUTH_TOKEN}" -XPOST https://perfectlabel.io/api/v001/fragments/ \
    -F img_original=@w.png
```

```javascript
{ 
   "img_unwrapped":{ 
      "id":51,
      "full_url":"https://perfect-label-uploads.s3.amazonaws.com/images/f164b064-dd11-4147-9910-ec7a49b8b0bc.jpg",
      "updated":"2020-01-23T14:52:45.844523Z",
      "created":"2020-01-23T14:52:45.831022Z",
      "thumb_url":""
   },
   "index":0,
   "id":10,
   "updated":"2020-01-23T14:52:45.857247Z",
   "created":"2020-01-23T14:52:45.857213Z",
   "img_original":{ 
      "id":50,
      "full_url":"https://perfect-label-uploads.s3.amazonaws.com/images/91b63ed0-7d39-436c-8833-44a29e11f9b0.png",
      "updated":"2020-01-23T14:52:42.795515Z",
      "created":"2020-01-23T14:52:42.780564Z",
      "thumb_url":""
   }
}
```

## Stitching
For stitching functionality, it's required to create labels, and add fragments to it. Labels must be created in a given project.
User will have a default project created automatically (use list projects API to get your default project id).

### List Projects
```bash
$ export AUTH_TOKEN=<YOUR_TOKEN_HERE>
$ curl -H "Authorization: Token ${AUTH_TOKEN}" https://perfectlabel.io/api/v001/projects/

```


### Create Project
To create a new project, use the following API.

```bash
$ curl -H "Authorization: Token ${AUTH_TOKEN}" https://perfectlabel.io/api/v001/projects/ \
    -XPOST -H "Content-Type: application/json" \
    -d '{"name": "project name"}'
```

```javascript
{"id":4,"my_role":"admin","name":"project name"}
```

### Update Project
To update project parameters, use "PATCH" request:

```bash
$ curl -H "Authorization: Token ${AUTH_TOKEN}" https://perfectlabel.io/api/v001/projects/4/ \
    -XPATCH -H "Content-Type: application/json" \
    -d '{"name": "project name"}'
```

```javascript
{"id":4,"my_role":"admin","name":"project name"}
```

### Create Label
Create a new label inside project using the following API.

```bash
$ export PROJECT_ID=4  # set your project id from previous query
$ curl -H "Authorization: Token ${AUTH_TOKEN}" https://perfectlabel.io/api/v001/projects/${PROJECT_ID}/labels/ \
    -XPOST -H "Content-Type: application/json" \
    -d '{"pos_id": "label unique identifier"}'
```

```javascript
{ 
   "updated":"2020-01-22T12:41:35.914052Z",
   "pos_id":"label unique identifier"',
   "id":1,
   "description":"",
   "status":"waiting_for_pictures",
   "stitched_image":null,
   "created":"2020-01-22T12:41:35.914015Z"
}
```

**NOTE**: you can omit `pos_id`

### List Project Labels
List labels of the project, the available label fields: id, status

```bash
$ export PROJECT_ID=4
$ curl -H "Authorization: Token ${AUTH_TOKEN}" https://perfectlabel.io/api/v001/projects/${PROJECT_ID}/labels/?label_fields=id,status,created,stitched_image&image_fields=id,full_url
```

### Get Label
Get label information, the available label fields: id, status
```bash
$ export PROJECT_ID=4
$ export LABEL_ID=11 
$ curl -H "Authorization: Token ${AUTH_TOKEN}" https://perfectlabel.io/api/v001/projects/${PROJECT_ID}/labels/${LABEL_ID}/?label_fields=id,status,created,stitched_image&image_fields=id,full_url
```

### Add Fragment
To add fragments to the given label, use the following API:

```bash
$ export LABEL_ID=11
$ curl -H "Authorization: Token ${AUTH_TOKEN}" -XPOST https://perfectlabel.io/api/v001/projects/${PROJECT_ID}/labels/${LABEL_ID}/fragments/ -F img_original=@w.png
```

```javascript
{ 
   "img_unwrapped":{ 
      "updated":"2020-01-22T12:44:31.708888Z",
      "full_url":"https://perfect-label-uploads.s3.amazonaws.com/images/a95a83ae-c0d5-4ae5-b4e6-dc423f308e01.jpg",
      "id":32,
      "thumb_url":"",
      "created":"2020-01-22T12:44:31.691055Z"
   },
   "id":3,
   "img_original":{ 
      "updated":"2020-01-22T12:44:23.804411Z",
      "full_url":"https://perfect-label-uploads.s3.amazonaws.com/images/64e03d6b-ab7d-4871-bf03-289a04605670.png",
      "id":31,
      "thumb_url":"",
      "created":"2020-01-22T12:44:23.791550Z"
   },
   "updated":"2020-01-22T12:44:31.721921Z",
   "index":0,
   "created":"2020-01-22T12:44:31.721855Z"
}
```
The example above will take a picture of the bottle, saves as "w.png", upload it to the server, detect label, and unwrap it automatically.

### Stitch Fragments

Once all the fragments uploaded, use the following API to stitch them:

```bash
$ curl -H "Authorization: Token ${AUTH_TOKEN}" -XPOST https://perfectlabel.io/api/v001/projects/${PROJECT_ID}/labels/${LABEL_ID}/stitch/
```

```javascript
{ 
   "pos_id":"",
   "description":"",
   "updated":"2020-01-22T13:31:08.349043Z",
   "stitched_image":{ 
      "created":"2020-01-22T13:31:08.337820Z",
      "thumb_url":"",
      "full_url":"https://perfect-label-stitch.s3.amazonaws.com/stitched/22-01-2020-fb78978a-4ed7-43b2-bcdc-9057d37b01a0.jpg",
      "updated":"2020-01-22T13:31:08.337900Z",
      "id":39
   },
   "created":"2020-01-22T12:41:35.914015Z",
   "status":"complete",
   "id":1
}
```

### List Fragments
List fragments of the label

https://perfectlabel.io/api/v001/projects/${PROJECT_ID}/labels/${LABEL_ID}/fragments/?fragment_fields=id,created,img_original,img_unwrapped&image_fields=id,full_url
```javascript
{
    "count": 1,
    "next": null,
    "previous": null,
    "results": [
        {
            "created": "2020-04-02T11:05:47.720523Z",
            "id": 516,
            "img_original": {
                "full_url": "https://perfect-label-uploads.s3.amazonaws.com/images/6d35d25b-22ce-45a5-aa87-fea59a73a2c6.jpg",
                "id": 1076
            },
            "img_unwrapped": {
                "full_url": "https://perfect-label-uploads.s3.amazonaws.com/images/5bd9c8e2-d0ef-41e6-ad61-4f3adf88d330.jpg",
                "id": 1077
            }
        }
    ]
}
```