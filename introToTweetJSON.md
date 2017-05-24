## Introduction to Tweet JSON

When ingesting Tweet data the main object is the Tweet Object, which is a parent object to many child objects. For example, all Tweets include a User object that describe who authored the Tweet. If you are working with an oject is a Retweet or Quoted Tweet, then that object will contain two Tweet objects, complete with two User objects. If the Tweet is geo-tagged, there will be location and place objects included. Every Tweet includes a entities object that encapsulates arrays of hashtags, user mentions, URLs, cashtags, and native media. If the Tweet has at lease one native photo, then there is an extended_entities object with metadata for up to four photos.

These objects are encoded using JavaScript Object Notation (JSON). JSON describes objects as a hierachy of objects and attributes.

< Key/value pairs >

```
created_time
text
user.screen_name
```

< Note about Tweet JSON Formats: Native and Activity Stream>


## Tweet Object 

+ [Tweet](#tweet) - Also referred to as a 'Status' object, 'root-level' attributes, _parent_ of other objects.

## Retweet and Quoted Tweet Objects

When a JSON payload represents a Retweet or Quoted Tweet, then there are Tweet objects included, complete with two User objects, with this addtional Tweet metadata provided:

  + Retweeted Status - Contains the original Tweet, the one that was Retweeted.
  + Quoted Status - Contains the original Tweet, the one that was Quoted.

    
+ Retweeted Status - Contains the original Tweet, the one that was Retweeted.
+ Quoted Status - Contains the original Tweet, the one that was Quoted.
  
Check out this article on identifying Retweets.
  
## Child objects.
  
  + [User](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/User_JSON_Native.md) - Twitter Account level metadata.
  + [Entities](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/Entities_JSON_Native.md) - Contains arrays of #hashtags, @mentions, $symbols, URLs, and media.
  + [Extended Entities](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/Entities_JSON_Native.md) - Contains up to four native photos.
  
When a Tweet has been geo-tagged with either an exact location or a Twitter Place, these objects are of interest:
  + Places
  + Coordinates
 
  
## Tweet JSON Examples
  
  
## Next Steps

+ Data Dictionaries
+ Learn more about what JSON attribute filters are available in Twitter Products.
  
