+ [] Host where initially?
+ [] Update links to sub objects.
+ [] User playload link (remove from here?)
+ [] Would be good to have direct link to language Operator docs.

## Introduction to Tweet JSON

All Twitter APIs that return Tweets provide that data encoded using JavaScript Object Notation (JSON). This notation is based on key-value pairs, with named attributes and associated values. These attributes are used to describe objects, and Tweets are made up of several fundamental objects. For a detailed introduction to how Tweets are encoded in JSON, see [HERE](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/introToTweetJSON.md).  <-- arriving soon?

When ingesting Tweet data the main object is the Tweet Object, which is a parent object to many child objects. For example, all Tweets include a User object that describe who authored the Tweet. If you are working with an object is a Retweet or Quoted Tweet, then that object will contain two Tweet objects, complete with two User objects. If the Tweet is geo-tagged, there will be location and place objects included. Every Tweet includes a entities object that encapsulates arrays of hashtags, user mentions, URLs, cashtags, and native media. If the Tweet has at lease one native photo, then there is an extended_entities object with metadata for up to four photos.

These objects are encoded using JavaScript Object Notation (JSON). JSON describes objects as a hierachy of objects and attributes.

< Key/value pairs >

```
created_at
text
user.screen_name
```

< Note about Tweet JSON Formats: Native and Activity Stream>


## Fundamental Objects

### Tweet Object 

+ [Tweet](#tweet) - Also referred to as a 'Status' object, 'root-level' attributes, _parent_ of other objects.

### Retweet and Quoted Tweet Objects

When a JSON payload represents a Retweet or Quoted Tweet, then there are Tweet objects included, complete with two User objects, with this addtional Tweet metadata provided:

  + Retweeted Status - Contains the original Tweet, the one that was Retweeted.
  + Quoted Status - Contains the original Tweet, the one that was Quoted.
 
Also, check out [this article on identifying Retweets and Quote Tweets](http://support.gnip.com/articles/identifying-and-understanding-retweets.html).
  
### Child objects.
  
  + [User](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/User_JSON_Native.md) - Twitter Account level metadata.
  + [Entities](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/Entities_JSON_Native.md) - Contains arrays of #hashtags, @mentions, $symbols, URLs, and media.
  + [Extended Entities](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/Entities_JSON_Native.md) - Contains up to four native photos.
  
When a Tweet has been geo-tagged with either an exact location or a Twitter Place, these objects are of interest:
  + Places
  + Coordinates
 

## Data Dictionaries

<Map out the documentation>

The following documentation provides *Data Dictionaries* for fundamental Twitter objects that make up a Tweet. These fundamental objects include the Tweet (*parent*) object itself, along with several *child* objects, such as user, entities, and extended entities objects. 

These objects have a hierachry which informs the layout of these Data Dictionaries: 
+ [Tweet](#tweet) - Also referred to as a 'Status' object, 'root-level' attributes, _parent_ of other objects.
  + [User](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/User_JSON_Native.md) - Twitter Account level metadata.
  + [Entities](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/Entities_JSON_Native.md) - Contains arrays of #hashtags, @mentions, $symbols, URLs, and media.
  + [Extended Entities](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/Entities_JSON_Native.md) - Contains up to four native photos.  
  + [Places]()
    + [Coordinates]() 
    
### Tweets, Retweets and Quoted Tweets   
  
When a JSON payload represents a Retweet or Quoted Tweet, then there are Tweet objects included, complete with two User objects, with this addtional Tweet metadata provided:
 + Retweeted Status - Contains the original Tweet, the one that was Retweeted.
 + Quoted Status - Contains the original Tweet, the one that was Quoted.

### Product Details

These JSON attribute dictionaries are specifically for the Tweets delivered by the following Twitter products:
+ Twitter Firehose 
+ Real-time PowerTrack
+ Historical PowerTrack
+ Twitter Search APIs (Full-Archive Search and 30-Day Search)

Please note that Tweets sourced elsewhere may vary somewhat in structure from this document.

Consumers of Tweets should tolerate the addition of new fields and variance in ordering of fields with ease. Not all fields appear in all contexts. It is generally safe to consider a nulled field, an empty set, and the absence of a field as the same thing. 
  
### Parsing Tips

Consumers of ```entities``` and ```entities_extended``` sections must be tolerant of 'missing' fields, since not all fields appear in all contexts. Parsers should tolerate the addition of new fields and variance in ordering of fields with ease. It is generally safe to consider a nulled field, an empty set, and the absence of a field as the same thing.
 
  
### Next Steps:  
+ Tweet object and data dictionary
+ User object and data dictionary
+ Entities and Extended Entities objects and data dictionaries
+ Tweet location objects and data dictionaries
+ For information on what PowerTrack Operators match on what JSON metadata, and what Operators are available in what Twitter products, see [HERE](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/metadataEvolution/operatorJSON.md).
+ Learn more about what JSON attribute filters are available in Twitter Products.
  
