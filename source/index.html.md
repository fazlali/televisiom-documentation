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

Welcome to Televisiom API documentation!

## Libraries

### Javascript library
This the library that both the developers and end users need to include in their web pages.

Features:

* Authorization
* Upload and Retrieve videos
* Set poster for video
* Add/Remove subtitle to/from video
 
For using Televesiom javascript library, jQuery framework must be included first:

`<script src="jquery.min.js"></script>`

then include Televisiom library:

 `<script src="https://televisiom.ir/js/video-api.js"></script>`
 
now it is possible to use `VideoApi` class in javascript codes.

# Types
All the objects that a developer may get involved in Televisom project, include Video, Poster, Thumbnail,Quality and Subtitle
## Video
an object that represent the actual video file with all other related elements like poster, thumbnail...
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

A json representation of Video object looks like this:

Name | Type | Always | Description
-----| -----| ------ | -----------
token | string | yes | The token of video that identifies video on server
duration | integer | yes | Duration of video in seconds
posters | array of [poster objects](#poster) | yes | Candidate posters from video to choose from and insert in 'poster' field
poster | [poster object](#poster) | yes | Selected poster for video
thumbnails | array of [thumbnail objects](#thumbnail) | yes | Thumbnails of video for preview
qualities | array of [quality objects](#quality) | no | Different resolution qualities of video
subtitles | array of [subtitle objects](#subtitle) | no | Different subtitles for video



## Poster
The poster object specifies an image to be shown while the video is previewed, or until the user hits the play button.
Each poster is a frame of video.
> Sample poster object

```json
{
  "id": 10,
  "time": 520,
  "image": "https://televisiom.ir/----.jpg",
  "thumbnail": "https://televisiom.ir/----.jpg"
}
```

A json representation of Poster object looks like this:

Name | Type | Always | Description
-----| -----| ------ | -----------
id | integer | yes | the id of poster that identifies poster on server
time | integer | yes | time of the extracted frame on video
image | string | yes | full url of the poster file
thumbnail | string | yes | full url of poster's thumbnail file

## Thumbnail

> Sample poster object

```json
{
  "id": 20,
  "time": 340,
  "image": "https://televisiom.ir/----.jpg"
}
```
Video thumbnails let viewers see a quick snapshot of your video.
Each thumbnail is a from of the video. 

A json representation of Thumbnail object looks like this:

Name | Type | Always | Description
-----| -----| ------ | -----------
id | integer | yes | the id of thumbnail that identifies thumbnail on server
time | integer | yes | the exact time of the extracted frame from video
image | string | yes | full url of thumbnail file

## Quality

After each video in uploaded to the server, different qualities of that video will be
generated and stored.

> Sample quality object

```json
{
  "width": 1280,
  "height": 720,
  "file": "https://televisiom.ir/----.mp4"
}
```

A json representation of Quality object looks like this:

Name | Type | Always | Description
-----| -----| ------ | -----------
width | integer | yes | the resolution width of this video quality
height | integer | yes | the resolution height of this video quality
file | string | yes | full url of this quality of video file.

## Subtitle

Each video file may have different subtitles in different languages.
For uploading a new subtitle for a video, the uploader must upload a .srt or .vtt file.
After each subtitle upload the server will automatically generate the  missing .srt, .vtt and quotes.  

> Sample subtitle object

```json
{
  "language": {...},
  "quotes": [{...}, ...],
  "srt": "https://televisiom.ir/----.srt",
  "vtt": "https://televisiom.ir/----.vtt"
}
```

A json representation of subtitle object looks like this:

Name | Type | Always | Description
-----| -----| ------ | -----------
language | [language object](#subtitle-language) | yes | information about language of the subtitle
quotes | array of [quote object](#subtitle-quote) | yes | quotes of the subtitle
srt | string | yes | full url of subtitle file by "SubRip text" syntax
vtt | string | yes | full url of subtitle file by "WebVTT" syntax

### subtitle language



> Sample subtitle language object

```json
{
  "code": "fa",
  "name": "Farsi"
}
```

A json representation of subtitle language object looks like this:

Name | Type | Always | Description
-----| -----| ------ | -----------
code | string | yes | according to languages two letter representation(iso 639-1)
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

Each sentences of a subtitle, consists of a text which is shown in a specific period of the video.
After each subtitle upload for a video, server will automatically generate quotes for the video.
It is most useful for developers who want to implement their own video player.
A json representation of subtitle quote object looks like this:


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
