# Technical Requirements
1) System should work exactly like all other days
2) User should able to find latest news for a hashtag
3) User can subscribe to a hashtag
# Non Technical Requirements
1) Eventual Consistant
2) Highly Available
3) Highly Scalable
4) Fault tolerant
# API Design
1) [POST] /api/v1/posts
```
RequestBody
{
	"userID" : 10192429424,
	"imageID" : 32109409358,
	"text" : "this is a flower!!",
	"hashtags" : ["#pic","#flower"]
}

ResponseBody
{
	"ID": 234979868638
	"userID" : 10192429424,
	"imageID" : 32109409358,
	"text" : "this is a flower!!",
	"hashtags" : ["#pic","#flower"]
}
```
2) [GET] /api/v1/posts?hashtag=flower&page=1&limit=100
```
[
	{
		"ID": 234979868638
		"userID" : 10192429424,
		"imageID" : 32109409358,
		"text" : "this is a flower!!",
		"hashtags" : ["#pic","#flower"],
		"likes": 1500000,
		"comments": 500000
	},
	{
		"ID": 357869774
		"userID" : 724187230,
		"imageID" : 238478979832,
		"text" : "Wow!!",
		"hashtags" : ["#flower", "yellow"],
		"likes": 1000000,
		"comments": 400000
	}
	...
]
```
# High Level Design
![[instagram_hot_topic|1000]]