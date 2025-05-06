# Functional Requirements
1) Show suggestions based on usage (alternatives relevance, popularity, context)
2) Support multiple languages and contexts. (out of scope)
# Non Functional Requirements
1) Low latency
2) Highly Available
3) Highly Scalable
4) Personalization for individual users. (out of scope)
# API Design 
1) [GET] /api/v1/auto-suggest?text=I+hope+you+feel
```
{ 
	"text": "I+hope+you+feel",
	"suggestions":
	 [
	  {"rank": 1, "suggestion": "better+soon"},
	  {"rank": 2, "suggestion": "good"}
	 ]
}
```
2) [POST] /api/v1/auto-suggestion
```
{
	"text" : "I+hope+you+feel+alive"
}
```
# Estimation
```
Number of user: 1 billion
Number of active users using this app: 1 millions
RPM for the api: 1mil RPM
```
# High Level Design
![[auto_suggestion|1000]]

# Low Level Design

```
ClassDiagram
class Word {
	+Number id
	+String text
	+Number usage
	+Boolean withStop 
	+[]Word nextWords
}
```


# Alternative algorithm for suggestion engine
1) ML Scoring Model (RAG)
2) N-Gram Models (https://www.youtube.com/watch?v=GiyMGBuu45w)
3) Ranking Suggestions