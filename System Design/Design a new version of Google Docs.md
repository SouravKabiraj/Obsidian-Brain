# Functional Requirements
1) Create doc file 
2) Edit doc file
3) Realtime colaboration
4) Save doc file
5) View doc file
6) Delete doc file
7) Access Control [scope out]
8) Offline Editing [scope out]

# Non Functional Requirements
1) **High availability**
2) High consistency
3) **Low latency**
4) **Scalable**

# Estimations
1) Number of user per day 10 million
2) Number of docs created per day 5 million
3) Number of char per doc created per doc = 1000 chars
4) Total storage used = 1000 x 5 million byte = 5 million kB = 5GB

# Low Level Design
ClassDiagram 
Class Document {
	+ String ID
	+ String title
	+ String url
	+ String creator
	+ Time createdBy
	+ Time updatedBy
}

Class Page {
	+ String ID
	+ String pageNo
	+ String documentID (indexed)
	+ String content
	+ Time createdBy
	+ Time updatedBy
}

Class EditHistory {
	+ String ID
	+ String pageID
	+ String content
	+ String author
	+ Time createdBy
	+ Time updatedBy
}


# API Design
- **POST** /api/v1/document
- **GET** /api/v1/pages?documentID={}
- **PATCH** /api/v1/pages/:pageID    {"op": "update", "path": "content", value: "....siheir...."}
- **DELETE** /api/v1/document/:documentID

# High Level Diagram
![[google_doc|1000]]