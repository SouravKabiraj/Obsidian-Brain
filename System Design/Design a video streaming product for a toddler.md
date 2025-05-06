# Functional Requirements
1) Kids will able to view contents
2) Parents will able to control daily_view_time, contents
3) content onboarding process should allow only kids friendly contents
4) No Advertisement
# Non Functional Requirements
1) Scalable
2) Security
3) Available
4) Reliable
5) Eventual Consistency
6) Fault Tolerant
# Estimation
```
Number of kids = 10Mil
Number of Parents = 10Mil
Number of video upload per day = 10k
Avarage size of the video = 5 min
Avarage size of the video = 80mb
Additional video data everyday (one format) = 80mb x 10k = 800,000mb = 800GB = 1TB
Additional video data for everyday = 3TB
Number of active user per min = 500k
```
# API Design
```
[POST] /api/content/view/control
{
	"videolength" : {
		max:
		min:
	},
	"allowedContentType" : ["educational","poem"]
}

[PATCH] /api/manage/:id
{
	"op": "append",
	"path": "/kids"
	"value": "kidID2"
}

[GET] /api/view/:id

[POST] /api/upload

[GET] /api/user/activity/history
[
	{
		"activityID" : 101093,
		"type": "search",
		"target": "pogo"
	},
	{
		"activityID" : 2394923,
		"type": "watch",
		"target": "320403"
	}
]
```

# Low Level Design
![[youtube_kids|1000]]
# High Level Design
![[youtube_kid_hld|1000]]