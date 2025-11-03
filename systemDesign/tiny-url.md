#systemDesign 

### Approaching a system design problem

![[Pasted image 20251103193332.png]]

##### Functional Requirements
1) Core features of the system, identify top 2-3 features that we will design during the interview. Ask lot of questions in first step to understand the question properly
	-  create a short url from a long url
		- optionally support custom alias
		- optionally support an expiration time
	- be redirected to the original url from the short url

##### Non-functional Requirements
1) these are the qualities of the system. Like Scalability, latency, fault tolerance, durability, security, compliance, CAP theorem etc.
	- Low latency on redirects ( ~200ms )
	- Scale to support 100M DAU and 1B urls
	- Ensure uniqueness of the short codes
	- High availability, eventual consistency for url shortening
	- 

#### Core Entities
1) These are basically the tables in db. No need of detailed models here, just list them down here
	- Original url
	- short url
	- user

#### API
1) In most cases, these are in one to one mapping with the functional requirements
	- // shorten a url
		- POST /urls -> shortUrl
		 {originalUrl, alias?, expirationTime?}
	- // redirection
		- GET shortUrl -> Redirect to original Url

#### High level design
1) Here also, are primary goal is to satisfy the functional requirements and not to worry about the non-functional requirements 
2) Suggested way to do it: Come over each of the api and draw the system to satisfy each api
 ![[Pasted image 20251103214423.png]] 
#### Deep dives
1) Go through each of the non-functional requirements, evolve the HLD such that it meets each of the N-f-r

![[Pasted image 20251103231144.png]]