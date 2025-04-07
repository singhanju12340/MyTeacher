---
Creation Time: Wednesday, March 12th 2025
Modified Time: Wednesday, March 12th 2025
---
**  
  

Design website like stack over flow

Replay , comments

  
  

Requirement:

1. User should able to post their queries
    
2. Other users should be able to respond/comment to the queries
    
3. Users can upvote or downvote
    
4. Delete and update post and comment.
    
5. If deleted post delta comments as well
    
6. Post can have tags buildin or custom tags
    
7. Feed for user
    

  

Non-Functional

1. Read is most compared to post
    
2. Latency can take a hit.
    
3. data should not be lost
    
4. Page load should be fast: within 5 ms
    
5. Size of post should be restricted 10KB
    
6. Comment chain limit 10
    

  

Estimates:

1M post /per day = 1M/86000 = 10/sec

5M read per day = 50/sec

  

Entities:

  

User

{

interestTag:[tech, new]

userId

userName

}

Tag{

Name

id

}

  

User Post

{

userId

postId>>>> question Id

String post details

upVote

Downvote

Tag id

createtime

}

  

User  answers

{

userId

answerId

questionId>>>> question

String post details

upVote

Downvote

Tag id

createtime

}

  
  
  

User post comments

{

questionId

answerId

commentId

String comment

comments:[{createdTime, commentId}]

createtime

}

  
  
  
  
  

Ans:

{

postId

Ans

}

  
  
  

Q1-

 A2

A

AX, AY

B

BX, By,BZ

C

A3

A4

  
  
  

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfjTAgC2TqZoPSmU6eWmaQkX95j_SdvsTml_wWNU2lhtNfApK3fFTX6X-SKuy1QlcO_FfJatDNPn8_ekG5ZK_NufT8gUByrfZu5q_UrgQmvFpwHLqKEscyyBCZpBPkG3193TOqygw?key=m1YI8_6tLNDz94H2P5ew5kYz)

**


_**How do we maintain Nested comment hierarchy?

Adjacency list
Nested Set Model
Closure Table