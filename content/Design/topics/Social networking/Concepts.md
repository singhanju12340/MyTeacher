---
Creation Time: Sunday, January 19th 2025
Modified Time: Monday, January 20th 2025
---
1. Scale
2. No premature optimisations
3. Every component can fail


### Features:

Photo upload
hashtags
messagings
custom groups
stories
feed
ranking
reactions
search & discovery
photo edit
people tag
trending 
notification
people you may follow
image processing pipeline
exploring access patterns


### NFR
No data miss



## Image service

Add constraints about image size and type on FE.
1. upload image
2. verifies image and convert it into base64 data
3. `upload image and get url seperate api call then create post and save details in db in another api call`
 Or `upload image and post both action will be done in 1 step`. but in this approach we will not be able to save as draft and do not create post. also image first needs to be send to server  by https request , keep in server in memory then uploaded to blob/s3.
 






## Image service
once post is publised, image is saved in s3, then send it to queue to image optimiser
consumer consumes image it 
Generating multiple image with different resolution
keep different images with different name in s3 ex: uid-64.jpg, uid-128.jpg and upload in s3.

_Prob:   image fall back needs to managed, and there will be delay in image upload_
`Sol: `If request come to see latest post, fetch original from s3 and based on the client request generate lower format/regenerate image and upload it in S3 and return back to user and also cache this resolution in CDN for around avg days when posts are famous.
i.e On demand optimised image rendering
_how does client device load image based on speed?_
`Sol:` Based on the client request of height and width, or based on the last apis response time, we can use ping to check response time.



## HashTag tag  extraction service

 after analysing post and extracting tags, if we save tags in DB there will be too many write calls on DB. 
 EX: 8 tags avg  each post * 2M post daily  = 16 Million tags  
