# Functional Requirements
1) User's can upload videos (P0)
2) User's can view videos (P0)
3) User's can like videos
4) User's can comment on videos
5) Show recommendation for user (P0)
# Non Functional Requirements
1) Available
2) Scalable
3) Fault tolerance 
4) Minimal video load time
# Estimations
```
Number of active users = 1 billion
Avarage video uploads per day = 10K
Avarage number of video watch per day = 3 videos/day-per-user
Avarage number of video watch per day = 3 billion/day
Additional data each day = 10K * 50MB * 4 = video_counts x avg_size x diff_formats
= 2000GB = 2TB
Additional data each month = 2TB x 30 = 60TB
Read RPM = 2 Million
Write RPM = 7
```
# API Design 
1) [POST] /api/v1/content
```
Request Body
{
	"title": "Tutorial for Golang",
	"description": "....."
}
```
2) [POST] /api/v1/content/:ID/thumbnail
3) [POST] /api/v1/content/:ID/upload
4) [GET] /api/v1/homepage?userID={}
```
[
	{"id": 108024, "title": "Tutorial for Golang", "view": 100k, "image":"cdn_url"},
	....
]
```
5) [GET] /api/v1/content/108024
```
{ 
	"id": 108024, 
	"title": "Tutorial for Golang", 
	"description": "Tutorial for Golang....", 
	"view": 100k, 
	"like": 10k, 
	"video":"cdn_url"
}
```
6) [GET] {cdn}/videoplayback?contentid={}&rn={}&source=youtube
# High Level Design 
![[youtube_system_design|1000]]
# Low Level Design 
```
class Content {
	+String ID [PK]
	+String Title
	+String Description
	+String ContentUrl
	+String ThumbnailUrl
}
class Like {
	+String ID [PK]
	+String UserID [FK]
	+String TargetID [FK]
	+String TypeID
}
class Comment {
	+String ID [PK]
	+String UserID [FK]
	+String Text
	+String TargetID [FK]
	+String TypeID
}
```