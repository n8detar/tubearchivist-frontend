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

## Playlists Item View
/api/playlist/\<playlist_id>/

## Download Queue List View
/api/download/

### Add list of videos to download queue
POST /api/download/
```json
{
    "data": [
        {"youtube_id": "NYj3DnI81AQ", "status": "pending"}
    ]
}
```

## Download Queue Item View
/api/download/\<video_id>/


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
