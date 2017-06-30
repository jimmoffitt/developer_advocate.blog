+ [X] Host where initially? http://support.gnip.com/sources/twitter/dictionaries/tweet_json_intro.html
+ [] Update links to sub objects.
+ [] User payload link (remove from here?) 
+ [] Would be good to have direct link to language Operator docs. Are there other Operators that need their own link? Group classes of Operators too -- Profile Geo, Geo, Entities.

## Introduction to Tweet JSON

All Twitter APIs that return Tweets provide that data encoded using JavaScript Object Notation (JSON). This notation is based on key-value pairs, with named attributes and associated values. These attributes are used to describe objects. At Twitter we serve many objects as JSON, including Tweets, Users, and Location Metadata. These objects all encapsulate core attributes that describe the object. Each Tweet has an author, a message, a unique ID, a timestamp of when it was posted, and sometimes geo metdata shared by the user. Each User has a Twitter name, an ID, a number of followers, and most often an account bio. With each Tweet we also package up an 'entities' object, which is an array of common Tweet contents such as hashtags, mmentions, media, and links. If there are links, the JSON payload can also provide metadata such as the fully unwound URL and the webpage's title.

https://twitter.com/TwitterDev/status/850006245121695744

The following JSON illustrates the structure for these objects and some of their attributes:

```json
{
	"tweet": {
		"created_at": "Thu Apr 06 15:24:15 +0000 2017",
		"id_str": "850006245121695744",
		"text": "1\/ Today we\u2019re sharing our vision for the future of the Twitter API platform!\nhttps:\/\/t.co\/XweGngmxlP",

		"user": {
			"id": 2244994945,
			"name": "Twitter Dev",
			"screen_name": "TwitterDev",
			"location": "Internet",
			"url": "https:\/\/dev.twitter.com\/",
			"description": "Your official source for Twitter Platform news, updates & events. Need technical help? Visit https:\/\/twittercommunity.com\/ \u2328\ufe0f #TapIntoTwitter"   
		},
		"place": {

		},
		"entities": {
			"hashtags": [],
			"urls": [{
				"url": "https:\/\/t.co\/XweGngmxlP",
				"unwound": {
					"url": "https:\/\/cards.twitter.com\/cards\/18ce53wgo4h\/3xo1c",
					"title": "Building the Future of the Twitter API Platform"
				}
			}],
			"user_mentions": []
		}
	}
}
```

## Twitter Objects and Data Dictionaries

When ingesting Tweet data the main object is the Tweet Object, which is a parent object to several child objects. For example, all Tweets include a User object that describe who authored the Tweet. If the Tweet is geo-tagged, there will be location and place objects included. Every Tweet includes a entities object that encapsulates arrays of hashtags, user mentions, URLs, cashtags, and native media. If the Tweet has any 'attached' or 'native' media (photos, video, animated GIF), there will be an extended_entities object.

```json
{
   "tweet": {
	"user": {},
	"place": {},
	"entities": {},
	"extended_entities": {}
   }
}
```

If you are working with an object is a Retweet or Quoted Tweet, then that object will contain two Tweet objects, complete with two User objects. 


```json
{
	"tweet": {
		"user": {},
		"retweeted_status": {
			"tweet": {
				"user": {},
				"place": {},
				"entities": {},
				"extended_entities": {}
			},
			"place": {},
			"entities": {},
			"extended_entities": {}
		}
	}
}
```

What is a data dictionary, how are they helpful?  What information do they give me?

These objects have a hierachry which informs the layout of these Data Dictionaries: 
+ [Tweet](#tweet) - Also referred to as a 'Status' object, 'root-level' attributes, _parent_ of other objects.
  + [User](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/User_JSON_Native.md) - Twitter Account level metadata.
  + [Entities](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/Entities_JSON_Native.md) - Contains arrays of #hashtags, @mentions, $symbols, URLs, and media.
  + [Extended Entities](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/Entities_JSON_Native.md) - Contains up to four native photos.  
  + [Places]()
    + [Coordinates]() 
    







### Tweet Object 
+ [Tweet](#tweet) - Also referred to as a 'Status' object, 'root-level' attributes, _parent_ of other objects.
  
### Tweet child objects.
 
  + [User](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/User_JSON_Native.md) - Twitter Account level metadata.
  + [Entities](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/Entities_JSON_Native.md) - Contains arrays of #hashtags, @mentions, $symbols, URLs, and media.
  + [Extended Entities](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/Entities_JSON_Native.md) - Contains up to four native photos.
  
When a Tweet has been geo-tagged with either an exact location or a Twitter Place, these objects are of interest:
  + Places
  + Coordinates

### Retweet and Quoted Tweet Objects

When a JSON payload represents a Retweet or Quoted Tweet, then there are Tweet objects included, complete with two User objects, with this addtional Tweet metadata provided:

  + Retweeted Status - Contains the original Tweet, the one that was Retweeted.
  + Quoted Status - Contains the original Tweet, the one that was Quoted.
 
Also, check out [this article on identifying Retweets and Quote Tweets](http://support.gnip.com/articles/identifying-and-understanding-retweets.html).

The following documentation provides *Data Dictionaries* for fundamental Twitter objects that make up a Tweet. These fundamental objects include the Tweet (*parent*) object itself, along with several *child* objects, such as user, entities, and extended entities objects. 



## Product Details

These JSON attribute dictionaries are specifically for the Tweets delivered by the following Twitter products:
+ Twitter Firehose 
+ Real-time PowerTrack
+ Historical PowerTrack
+ Twitter Search APIs (Full-Archive Search and 30-Day Search)

Please note that Tweets sourced elsewhere may vary somewhat in structure from this document.

Consumers of Tweets should tolerate the addition of new fields and variance in ordering of fields with ease. Not all fields appear in all contexts. It is generally safe to consider a nulled field, an empty set, and the absence of a field as the same thing. 
  
## Parsing Best-practices

JSON parsers must be tolerant of 'missing' fields, since not all fields appear in all contexts. Parsers should tolerate the addition of new fields and variance in ordering of fields with ease. It is generally safe to consider a nulled field, an empty set, and the absence of a field as the same thing.
 
## Tweet JSON Formats 
 
< Note about Tweet JSON Formats: Native and Activity Stream> 
  
## Next Steps:  
+ Tweet object and data dictionary
+ User object and data dictionary
+ Entities and Extended Entities objects and data dictionaries
+ Tweet location objects and data dictionaries
+ For information on what PowerTrack Operators match on what JSON metadata, and what Operators are available in what Twitter products, see [HERE](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/metadataEvolution/operatorJSON.md).
+ Learn more about what JSON attribute filters are available in Twitter Products.
  
