# TubeArchivist API
Documentation of available API endpoints.  
**Note: This is very early alpha and will change!**

## Authentication
API token will get automatically created, accessible on the settings page. Token needs to be passed as an authorization header with every request. Additionally session based authentication is enabled too: When you are logged into your TubeArchivist instance, you'll have access to the api in the browser for testing.

Curl example:
```shell
curl -v /api/video/<video-id>/ \
    -H "Authorization: Token xxxxxxxxxx"
```

Python requests example:
```python
import requests

url = "/api/video/<video-id>/"
headers = {"Authorization": "Token xxxxxxxxxx"}
response = requests.get(url, headers=headers)
```

## Pagination
The list views return a paginate object with the following keys:
- page_size: int current page size set in config
- page_from: int first result idx
- prev_pages: array of ints of previous pages, if available
- current_page: int current page from query
- max_hits: reached: bool if max of 10k results is reached
- last_page: int of last page link
- next_pages: array of ints of next pages
- total_hits: int total results

Pass page number as a query parameter: `page=2`. Defaults to *0*, `page=1` is redundant and falls back to *0*. If a page query doesn't return any results, you'll get `HTTP 404 Not Found`.

## Login View
Return token and user ID for username and password:  
POST /api/login
```json
{
    "username": "tubearchivist",
    "password": "verysecret"
}
```

after successful login returns 
```json
{
    "token": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "user_id": 1
}
```

## Video List View
/api/video/

## Video Item View
/api/video/\<video_id>/

## Video Progress View
/api/video/\<video_id>/progress  

Progress is stored for each user.

### Get last player position of a video
GET /api/video/\<video_id>/progress
```json
{
    "youtube_id": "<video_id>",
    "user_id": 1,
    "position": 100
}
```

### Post player position of video
POST /api/video/\<video_id>/progress
```json
{
    "position": 100
}
```

### Delete player position of video
DELETE /api/video/\<video_id>/progress  


## Sponsor Block View
/api/video/\<video_id>/sponsor/

Integrate with sponsorblock

### Get list of segments
GET /api/video/\<video_id>/sponsor/


### Vote on existing segment
**This only simulates the request**  
POST /api/video/\<video_id>/sponsor/
```json
{
    "vote": {
        "uuid": "<uuid>",
        "yourVote": 1
    }
}
```
yourVote needs to be *int*: 0 for downvote, 1 for upvote, 20 to undo vote

### Create new segment
**This only simulates the request**  
POST /api/video/\<video_id>/sponsor/
```json
{
    "segment": {
        "startTime": 5,
        "endTime": 10
    }
}
```
Timestamps either *int* or *float*, end time can't be before start time.


## Channel List View
/api/channel/

### Subscribe to a list of channels
POST /api/channel/
```json
{
    "data": [
        {"channel_id": "UC9-y-6csu5WGm29I7JiwpnA", "channel_subscribed": true}
    ]
}
```

## Channel Item View
/api/channel/\<channel_id>/

## Channel Videos View
/api/channel/\<channel_id>/video/

## Playlist List View
/api/playlist/

## Playlists Item View
/api/playlist/\<playlist_id>/

## Playlist Videos View
/api/playlist/\<playlist_id>/video/

## Download Queue List View
GET /api/download/

Parameter:
- filter: pending, ignore

### Add list of videos to download queue
POST /api/download/
```json
{
    "data": [
        {"youtube_id": "NYj3DnI81AQ", "status": "pending"}
    ]
}
```

### Delete download queue items by filter
DELETE /api/download/?filter=ignore  
DELETE /api/download/?filter=pending

## Download Queue Item View
GET /api/download/\<video_id>/  
POST /api/download/\<video_id>/

Ignore video in download queue:
```json
{
    "status": "ignore"
}
```

Add to queue previously ignored video:
```json
{
    "status": "pending"
}
```

DELETE /api/download/\<video_id>/  
Forget or delete from download queue

## Ping View
Validate your connection with the API  
GET /api/ping

When valid returns message with user id: 
```json
{
    "response": "pong",
    "user": 1
}
```
