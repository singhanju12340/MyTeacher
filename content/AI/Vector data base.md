
A database to store index and embeddings of text, image and videos to find out similar content. 

#Embeddings are numeric representation of text, image and videos.



#Semantic_search: Not a streightforward keyword search its a search by understanding the intent of user query


### Sample of embeddings 

| Vector context key | Revenue of Apple | Calories in Apple | Nutrition in Orange |
| ------------------ | ---------------- | ----------------- | ------------------- |
| related_to_phone   | 1                | 0                 | 0                   |
| is_location        | 0                | 0                 | 0                   |
| has_stock          | 1                | 0                 | 0                   |
| revenue            | 80               | 0                 | 0                   |
| is_fruit           | 0                | 1                 | 1                   |
| calories           | 0                | 95                | 80                  |

If we see vector value of calories in apple and nutrition in orange. they have a matching values of is_fruit. the similarities are calculated based on the similarities of values in of the vector of text context.


There are different technics of generating vector embeddings. like Word2Vec, BERT, GPT, ELMo, Fast text etc

