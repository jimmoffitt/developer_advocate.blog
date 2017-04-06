
## Twitter Data timeline <a id="twitterTimeline" class="tall">&nbsp;</a>  

tl;dr

"Understanding how Twitter evolved is helpful since new features map directly to JSON 'objects' and their attributes. Queries composed of PowerTrack Operators enable you to find Tweets of interest by matching on these attributes." 

+ [Introduction](#introduction)
+ [Fundamental Metadata](#fundamentalMetadata)
+ [Twitter timeline](#twitterTimeline)
+ [Next Steps](#nextSteps)


### Introduction <a id="=introduction" class="tall">&nbsp;</a>

At its core, Twitter is a public, real-time, and global communication network. Since 2006, Twitter's evolution has been driven by both user use-patterns [ or conventions? ] and new product features and enhancements. If you are using Twitter data for historical research, understanding these evolutions is important for building effective queries of the Tweet archive. Twitter makes available two historical APIs that provide access to every publicly available Tweet: Historical PowerTrack and the Full-Archive Search API. 



#### From new user conventions to official Twitter features 

Twitter users organically introduced new, and now fundamental, communication patterns to the Twitter network. A seminal example is the hashtag, now nearly universally used across all social networks. Hashtags were introduced as a way to organize conversations and topics. On a network with hundreds of million messages a day, tools to find Tweets of interest are key, and hashtags have become a fundamental to find Tweets of interest. Soon after the use of hashtags grew, they recieved official status and support from Twitter. As hashtags became a 'first-class' *object*, this meant many things. It meant hashtags became clickable/searchable in the Twitter.com user interface. It also meant hashtags became a member of the Twitter *entities* family, along with @mentions, attached media, stock symbols, and shared links. These entities are conveniently encoded in a pre-parsed JSON array, making it easier for developers to process, scan, and store them. 

Retweets are another example of user-driven conventions becoming official objects. Retweeting emerged as a way of 'forwarding' content to others. It started as a manual process of copying/pasting a Tweet and prepending it with a "RT @" pattern. This process was eventually automated via a new Retweet button, complete with new JSON metadata. The 'official' Retweet was born. Other examples include 'mentions', sharing of media and web links, and sharing a location with your Tweet. Each of these use-patterns resulted in new [twitter.com](https://twitter.com/) user-interface features, new supporting JSON, and thus new ways to match on Tweets. All of these fundamental Tweet attributes have resulted in PowerTrack Operators used to match on them.

#### From a SMS mobile app to a communication platform

Since 2006, Twitter has also evolved as a *Platform*, complete with a rich set of APIs. APIs have always been a pillar of the Twitter network. The [first API hit the streets soon after Twitter was launched](https://blog.twitter.com/2006/introducing-the-twitter-api). When geo-tagging Tweets was first introduced in 2009, it was made available through a [Geo API](https://blog.twitter.com/2009/think-globally-tweet-locally) (and later became a 'first-class' Twitter.com user-interface feature). 



#### From a Tweet to JSON to matching *Operators*

Today, Twitter's APIs drive the two-way communication network that has become the source of breaking news. The opportunities to build on top of this global, real-time communication channel are endless. If your use-case includes a need to *listen* to what the world is saying on Twitter, it is key to be able to find your Tweets of interest. it is important to understand how Twitter APIs match on the Tweet metadata. 

[a fine place to introduce how PowerTrack Operators **match** JSON attributes. ] 
Both of these APIs provide a querying syntax used to find Tweets of interest. 


match on Tweets and their supporting metadata. Queries are built with *operators* that enable you to find Tweets of interest based on Tweet message text, language, location, language, 


```
[] TODO
No need to go into other Platform features, e.g. DMs and Ads.
So, what are we covering next?
* We'll start off with more background about the evolution of fundamental Twitter characteristics, starting with Retweets and then collections of Twitter **entities**.
* Then a twitter timeline, along with comments on effects on JSON and query Operators. 
```

### Fundamental Metadata <a id="=fundamentalMetadata" class="tall">&nbsp;</a>

#### Retweets

```
[] TODO
* Retweets have an interesting history on Twitter. 
* user convention --> UI features --> JSON affects --> filtering details. 
* Beta period --> Official Retweet object with verb, original payload, and select metrics. 
```

#### Twitter Entities

```
[] TODO
* Hashtags, mentions, urls, media
* As these entities became first-class objects they became members of the *entities* collection of Tweet JSON attributes. Providing these arrays make it easier for JSON parsers to load these common Tweet entities by not requiring elaborate string parsing and extraction.  
```

### Twitter timeline <a id="=twitterTimeline" class="tall">&nbsp;</a>
Below you will find a *timeline* of Twitter (as a Product and Platform). Most of these Twitter updates in some way fundamental affected either user behavior, Tweet JSON contents, query Operators, or all three.  Looking at Twitter as a platform, the following events in some way affected the JSON payloads that are used to encode Tweets. In turn, those JSON details affect how Twitter historical API match on them. If you want to dig into those Twitter product details, see our documentation for [Full-Archive Search](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/metadataEvolution/search_timeline.md) and [Historical PowerTrack](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/metadataEvolution/hpt_timeline.md). 

#### 2006
+ October 
    - @replies becomes a convention.  
    - $cashtags first emerge, but using for stock ticker mentions does not become common until early 2009.
+ November - Favorites introduced. 

#### 2007
+ January - @replies become a first-class object with a UI reply button with ```in_reply_to``` metadata. 
+ April - Retweets become a convention. 
+ August - #hashtags emerge as a primary tool for searching and organizing Tweets. 

#### 2009
+ February - $cashtags become a common convention for discussing stock ticker symbols. 
+ May - Retweet 'beta' is introduced with "Via @" prepended to Tweet body.
+ June - Verified accounts introduced. 
+ August - Retweets a first-class object with "RT @" pattern and new ```retweet_status``` metadata. 
+ October - List feature launched. 
+ November - [Tweet Geotagging API is launched](https://blog.twitter.com/2009/think-globally-tweet-locally), providing first method for users to share location via third-party apps. 

#### 2010
+ June - Twitter Places introduced for geo-tagging Tweets. 
+ August - Tweet button for web sites is launched. Made sharing links easier.  

#### 2011
+ May - Follow button introduced, making it easier to follow accounts associated with web sites.  
+ August - Native photos introduced. 

#### 2016
+ June - Quoted Retweets generally available. 
+ September - 'Native attachments' introduced with trailing URL not counted towards 140 characters ("dwm140, part 1").

#### 2017
+ April - 'Simplified Replies' introduced with replied to accounts not counted towards 140 characters ("dmw140, part 2"). 


### Next Steps

+ Choosing between Historical PowerTrack and Search API
+ Historical PowerTrack API: metadata and filtering timeline  
+ Search API: metadata and filtering timeline  
+ Getting Started with Tweet JSON

=======================================
