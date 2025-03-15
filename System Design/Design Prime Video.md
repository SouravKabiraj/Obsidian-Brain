# Functional Requirements
1) Content Search
2) Home Page and Content Category 
3) Content Streaming 
4) Resume Content Streaming
5) Recommendation

# Non Functional Requirements
1) 1 million concurrent users from different region
2) Low streaming latency
3) High availability
4) Consistent System

# Components
## Content
- Content
	- Movie
		- Trailer
		- Subtitle
	- Web Series
		- Episodes
		- Trailer
		- Subtitle
	- Category
	- Casts
## User
- UserActivity
	- Type
	- Target
	- Metadata
	- Time
- User 
	- Name
	- Country
	- Preferences
## Recommendations
- Recommendation
	- Match Rank
	- Category
	- ContentID

# Low Level Design
![[prime_video_lld|1000]]

# High Level Design
![[prime_video_hld|1000]]

![[prime_content_upload_workflow|2000]]
## Trade Offs

## Storage of Contents 
1) SFTP
2) S3 **
3) CDN *

## Storage for recommendation engine
1) Vector DB
2) Graph DB

## Recommendation engine algorithm
1) RAG System
2) Collaborative Filtering (Cosine Similarity, Pearson correlation, Jaccard similarity)

## Database for Content Service
1) SQL Type (MySQL)
2) NoSQL Type (MongoDB) *
3) Cache (Redis) for homepage contents *