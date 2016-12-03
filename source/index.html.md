---
title: Televisiom API Reference

language_tabs:
  - shell
  - javascript

toc_footers:
  - <a href='https://televisiom.ir/register'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Televiiom API!

## Libraries

### Javascript library

Features:

* authorize
* retrieve video from server
* set poster for video
* add/remove subtitle to/from video
 
For using televesiom javascript library you should include jQuery framework first:

`<script src="jquery.min.js"></script>`

then include televisiom library:

 `<script src="https://televisiom.ir/js/video-api.js"></script>`
 
now you can use `VideoApi` class in your javascript codes.

# Types

## Video

> Sample video object

```json
{
  "token": "dasi587dsff58s58ds",
  "duration": 3600,
  "poster": {...},
  "posters": [{...}, ...],
  "thumbnails": [{...}, ...],
  "qualities": [{...}, ...],
  "subtitles": [{...}, ...]
}
```

A json object by this fields:

Name | Type | Always | Description
-----| -----| ------ | -----------
token | string | yes | the token of video that identifies video on server
duration | integer | yes | duration of video in seconds
poster | [poster object](#poster) | yes | select poster for video
posters | array of [poster objects](#poster) | yes | candidates poster to choose
thumbnails | array of [thumbnail objects](#thumbnail) | yes | thumbnails of video for preview
qualities | array of [quality objects](#quality) | no | ...
subtitles | array of [subtitle objects](#subtitle) | no | ...



## Poster

> Sample poster object

```json
{
  "id": 10,
  "time": 520,
  "image": "https://televisiom.ir/----.jpg",
  "thumbnail": "https://televisiom.ir/----.jpg"
}
```

A json object by this fields:

Name | Type | Always | Description
-----| -----| ------ | -----------
id | integer | yes | the id of poster that identifies poster on server
time | integer | yes | time of the extracted frame on video
image | string | yes | url of poster file
thumbnail | string | yes | url of poster's thumbnail file

## Thumbnail

> Sample poster object

```json
{
  "id": 20,
  "time": 340,
  "image": "https://televisiom.ir/----.jpg"
}
```

A json object by this fields:

Name | Type | Always | Description
-----| -----| ------ | -----------
id | integer | yes | the id of thumbnail that identifies thumbnail on server
time | integer | yes | time of the extracted frame on video
image | string | yes | url of thumbnail file

## Quality

> Sample quality object

```json
{
  "width": 1280,
  "height": 720,
  "file": "https://televisiom.ir/----.mp4"
}
```

A json object by this fields:

Name | Type | Always | Description
-----| -----| ------ | -----------
width | integer | yes | the width of clip
height | integer | yes | the height of clip
file | string | yes | url of clip file

## Subtitle

> Sample subtitle object

```json
{
  "language": {...},
  "quotes": [{...}, ...],
  "srt": "https://televisiom.ir/----.srt",
  "vtt": "https://televisiom.ir/----.vtt"
}
```

A json object by this fields:

Name | Type | Always | Description
-----| -----| ------ | -----------
language | [language object](#subtitle-language) | yes | the width of clip
quotes | array of [quote object](#subtitle-quote) | yes | quotes of subtitle
srt | string | yes | url of subtitle file by "SubRip text" syntax
vtt | string | yes | url of subtitle file by "WebVTT" syntax

### subtitle language

> Sample subtitle language object

```json
{
  "code": "fa",
  "name": "Farsi"
}
```

A json object by this fields:

Name | Type | Always | Description
-----| -----| ------ | -----------
code | string | yes | ...
name | string | yes | ...

### subtitle quote

> Sample subtitle quote object

```json
{
  "from": 23.36,
  "to": 25.86155,
  "text": "Televisiom is the best.."
}
```

A json object by this fields:

Name | Type | Always | Description
-----| -----| ------ | -----------
from | float | yes | ...
to | float | yes | ...
text | string | yes | ...

# Authentication

>Sample structure of JWT header part

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```
>Sample structure of JWT payload part

```json
{
  "iss": "issuer", // It can be your application name
  "aud": "My video website", // The name of your application that entered in Televisiom dashboard.
  "sub": {  // Curent user informations
    "user": "1", // A string that user can be identified with
    "permissions": ["upload"] // An array of user permissions
  },
  "iat": 1480423250, // Issue timestamp
  "exp": 1480434050 // The token will not be valid after this unix timestamp
}
```

> You should put JWT token in the authorization header of all of your requests

```shell
curl -X POST \
    -H "Authorization: bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJpc3N1ZXIiLCJhdWQiOiJNeSB2aWRlbyB3ZWJzaXRlIiwic3ViIjp7InVzZXIiOiIxIiwicGVybWlzc2lvbnMiOlsidXBsb2FkIl19LCJpYXQiOjE0ODA0MjMzODIsImV4cCI6MTQ4MDQzNDE4Mn0.zqW8BvDaMfTq7g0A10Ou_PKMQjWg7gYUVtBsw2VlQMg" \
    https://televisiom.ir/api/videos
```

```javascript
var televisiom = new VideoApi('eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJpc3N1ZXIiLCJhdWQiOiJNeSB2aWRlbyB3ZWJzaXRlIiwic3ViIjp7InVzZXIiOiIxIiwicGVybWlzc2lvbnMiOlsidXBsb2FkIl19LCJpYXQiOjE0ODA0MjMzODIsImV4cCI6MTQ4MDQzNDE4Mn0.zqW8BvDaMfTq7g0A10Ou_PKMQjWg7gYUVtBsw2VlQMg');
```

For authorize your users to use video api you should issue a JWT access key.
JWT tokens needs a secret key for generation. you can get the secret key of your application from dashboard.


The generated token should be like this:

`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJpc3N1ZXIiLCJhdWQiOiJNeSB2aWRlbyB3ZWJzaXRlIiwic3ViIjp7InVzZXIiOiIxIiwicGVybWlzc2lvbnMiOlsidXBsb2FkIl19LCJpYXQiOjE0ODA0MjMzODIsImV4cCI6MTQ4MDQzNDE4Mn0.zqW8BvDaMfTq7g0A10Ou_PKMQjWg7gYUVtBsw2VlQMg`


# Videos
## Upload a video


```shell
curl -X POST \
    -H "Authorization: bearer <jwt_access_token>" \
    -F "clip=@movie.mp4" \
    https://televisiom.ir/api/videos
```

```javascript
var file = $('input[type="file"]').get(0).files[0];
televisiom.upload(file).progress(function(progress) {
    // Do somthing like showing progress of upload.
}).done(function(video) {
    // Retrive the video object tha server returns on success.
}).fail(function(data) {
    // Do somthing whene an error occurred.
});
```

> The above command returns a [Video object](#video).


This endpoint upload a video.

### HTTP Request

`POST https://televisiom.ir/api/videos`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
clip | true | video clip for upload in binary format

### Return value

[Video object](#video)

## Get a video



```shell
curl -X GET \
    -H "Authorization: bearer <jwt_access_token>" \
    https://televisiom.ir/api/videos/<videoToken>
```

```javascript
var videoToken = "the token string of video";
televisiom.setVideo(videoToken).done(function(video) {
    // Retrive the video object tha server returns on success.
}).fail(function(data) {
    // Do somthing whene an error occurred.
});
```

> The above command returns a [Video object](#video).


### HTTP Request

`GET https://televisiom.ir/api/videos/<videoToken>`

### Query Parameters

There is no required parameter for request.

### Return value

[Video object](#video)

# Posters
## Set poster



```shell
curl -X PUT \
    -H "Authorization: bearer <jwt_access_token>" \
    -F "poster=<posterId>" \
    https://televisiom.ir/api/videos/<videoToken>
```

```javascript
var posterId = televisiom.video.posters[1].id;
televisiom.setPoster(posterId).then(function(video) {
  
});

```

> The above command returns a [Video object](#video).


### HTTP Request

`PUT https://televisiom.ir/api/videos/<videoToken>`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
poster | true | the poster id 

# Subtitles
## Add a subtitle



```shell
curl -X POST \
    -H "Authorization: bearer <jwt_access_token>" \
    -F "language=<language>" \
    -F "subtitle=@subtitle.srt" \
    https://televisiom.ir/api/videos/<videoToken>/subtitles
```

```javascript
var file = $('input[type="file"]').get(0).files[0];
var languageCode = "fa";
televisiom.addSubtitle(file, languageCode).then(function(video) {
  
});

```

> The above command returns a [Video object](#video).


### HTTP Request

`POST https://televisiom.ir/api/videos/<videoToken>/subtitles`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
language | true | the two-digit code of subtitle language
subtitle | true | subtitle file in srt or vtt format

## remove a subtitle



```shell
curl -X DELETE \
    -H "Authorization: bearer <jwt_access_token>" \
    https://televisiom.ir/api/videos/<videoToken>/subtitles/<subtitleLanguage>
```

```javascript
var languageCode = televisiom.video.subtitles[0].language;
televisiom.removeSubtitle(languageCode).then(function(video) {
  
});

```

> The above command returns a [Video object](#video).


### HTTP Request

`DELETE https://televisiom.ir/api/videos/<videoToken>/subtitles/<subtitleLanguage>`

### Query Parameters

There is no required parameter for request.
