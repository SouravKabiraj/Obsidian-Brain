# Functional Requirements
1) User will able to see live comments in a live video
2) User will able to comment on a live video
# Non Functional Requirements
1) Highly Scalable, Comments per video = 10K
2) Eventual Consistent
3) Fast Response Time
4) Availability (Low Priority)

# API Design 
1) web_socket to fetch and post comments 
   - Other options
	   - Long polling (bad experience)
	   - Firebase DB (costly)

# Components

## Low Level Objects
Class Comment {
	+String ID
	+String VideoID
	+String text
	+String userID
}

Class LiveVideo {
	+String ID
	 +String UserID
}

Class User {
	+String ID
	 +String UserName
}

![[facebook_live_chat | 2000]]


# FeedBack
1) We can create sub group when a live video has 1 million or more users