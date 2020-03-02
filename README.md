# Perfectlabel Api
You need to get authorization token first.

When you get it you can check that it is working by:

```bash
$ export AUTH_TOKEN=<YOUR_TOKEN_HERE>
$ curl -H "Authorization: Token ${AUTH_TOKEN}" https://perfectlabel.io/api/v001/projects/

```

```javascript
{"count":0,"next":null,"previous":null,"results":[]}
```

If you don't get correct responses, verify that you've passed correct token by:

```bash
echo $AUTH_TOKEN
```


### Simple unwrap API.

If you don't want to stitch several images together, you don't need projects and labels API's and can go with simple unwrap API.


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



### Create project

```bash
$ curl -H "Authorization: Token ${AUTH_TOKEN}" https://perfectlabel.io/api/v001/projects/ \
    -XPOST -H "Content-Type: application/json" \
    -d '{"name": "project name"}'
```

```javascript
{"id":4,"my_role":"admin","name":"project name"}
```


### Create label

```bash
$ export PROJECT_ID=4  # set your project id from previous query
$ curl -H "Authorization: Token ${AUTH_TOKEN}" https://perfectlabel.io/api/v001/projects/${PROJECT_ID}/labels/ \
    -XPOST -H "Content-Type: application/json" \
    -d '{"pos_id": "label unique identifier"}'
```

```javascript
{ 
   "updated":"2020-01-22T12:41:35.914052Z",
   "pos_id":"label name"',
   "id":1,
   "description":"",
   "status":"waiting_for_pictures",
   "stitched_image":null,
   "created":"2020-01-22T12:41:35.914015Z"
}
```

**NOTE**: you can omit `pos_id`

### Upload fragment

When you want to upload file named `w.png` you will post it like that:

```bash
$ export LABEL_ID=1  # set your LABEL_ID from previous query
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

### Stitching fragments

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

## Other

```bash
export AWS_ACCESS_KEY_ID=$AWS_DATASET_KEY
export AWS_SECRET_ACCESS_KEY=$AWS_DATASET_SECRET
```

