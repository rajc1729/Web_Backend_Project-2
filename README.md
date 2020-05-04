# CPSC 449 Web Backend Engineering
## Project-2
### Project Members:
* Raj Chhatbar (chhatbarraj@csu.fullerton.edu) | Role: Dev 3
* Mohammed Kasim Panjri (kasimp@csu.fullerton.edu) | Role: Dev 1
* Harlik Shah (shahharlik@csu.fullerton.edu) | Role: Dev 2 


### Note
Here we have used Reddit API to retrieve posts from Reddit. 
The uuid used is generated using Python uuid module and then converted to base36 encoding similar to how Reddit generates id.
All other attributes are retrieved from the API itself.

Attributes for post database in DynamoDB
```
uuid (unique ID) | username | title | url | description | published (timestamp) sort_key | community_name
```
Attributes for vote database in Redis
```
uuid (unique ID) | score (upvote-downvote) sort_key | community_name | published (timestamp) sort_key
```

Names of communities available in database
```
csuf | news | Coronavirus | Python | computerscience | bitcoin
```

Total number of posts in database: 10,000

#### ---------------------Dev 3 - Aggregating posts and votes with a BFF---------------------------
1) Use this code for generating 1 instance each for post_db, post_api, vote_api and front_BFF
```shell script
foreman start -m post_db=1,post=1,vote=1,front=1
```

2) Use the following URL to get RSS feeds

  * The 25 most recent posts to a particular community
```
http://localhost:5000/get?n=25&community_name=csuf
```
  * The 25 most recent posts to any community
```
http://localhost:5000/get?n=25
```
  * The top 25 posts to a particular community, sorted by score
```
http://localhost:5000/get_sorted?n=25&community_name=csuf
```
  * The top 25 posts to any community, sorted by score
```
http://localhost:5000/get_sorted?n=25
```
  * The hot 25 posts to any community, ranked using Reddit’s “hot ranking” algorithm.
```
http://localhost:5000/get_hot?n=25
```

#### ---------------------Dev 2 - Porting to the voting microservice to Redis---------------------------


#### -----------------Dev 1 - Porting the posting microservice to Amazon DynamoDB Local----------------------
* Create a new post
```shell script
curl -i -X POST -H 'Content-Type:application/json' -d '{"title":"Test post", "description":"This is a test post", "username":"some_guy_or_gal", "community_name":"449", "uuid":"9H1TQXRQQ8JAL7HE1OSWN6K5Z", "published":"1588265108"}' http://localhost:5100/create
```
* Delete an existing post
```shell script
curl -i -X DELETE http://localhost:5100/delete?uuid=9H1TQXRQQ8JAL7HE1OSWN6K5Z&published=1588265108
```
* Retrieve an existing post
```shell script
curl -i http://localhost:5100/get?uuid=CFXBWE9BP5VO51HNA0DE1QNIV
```
* List n most recent posts to a particular community
```shell script
curl -i http://localhost:5100/get?n=10&community_name=csuf&recent=True
```
* List n most recent posts to any community
```shell script
curl -i http://localhost:5100/get?n=10&recent=True
```
* Retrieve multiple posts using a list of uuids
```shell script
curl -i -X POST -H 'Content-Type:application/json' -d '{"uuid":["CQHYO2LBB1GFRIYVTH28TUEMV", "BAOL4MNKJWB2L04BC48IMKE53", "BAOL4EZ1LALJXK49HTOL84FBR", "CYBDDVCRY049BOWC2G0U2432V", "C36AVEOBBYY9BVV6LQBF74H3R"]}' http://localhost:5100/get_uuids
```
## License
[MIT](https://choosealicense.com/licenses/mit/)
