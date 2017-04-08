
## Twitter Data timeline <a id="twitterTimeline" class="tall">&nbsp;</a>  

tl;dr

"Understanding how Twitter evolved is helpful since new features map directly to JSON 'objects' and their attributes. In most cases the introduction of these attributes led to PowerTrack Operators used to query for Tweets of interest using two Twitter historical APIs: Historical PowerTrack and Full-Archive Search."

---------------------------------------------

+ [Introduction](#introduction)
+ [Fundamental Metadata](#fundamentalMetadata)
+ [Twitter timeline](#twitterTimeline)
+ [Next Steps](#nextSteps)


### Introduction <a id="=introduction" class="tall">&nbsp;</a>

At its core, Twitter is a public, real-time, and global communication network. Since 2006, Twitter's evolution has been driven by both user use-patterns and conventions and new product features and enhancements. If you are using Twitter data for historical research, understanding the timeline of this evolution is important for surfacing Tweets of interest from the data archive. 

Twitter was launched as a simple SMS mobile app, and has grown into a comprehensive communication platform. A platform with a complete with a rich set of APIs. APIs have always been a pillar of the Twitter network. The [first API hit the streets soon after Twitter was launched](https://blog.twitter.com/2006/introducing-the-twitter-api). When geo-tagging Tweets was first introduced in 2009, it was made available through a [Geo API](https://blog.twitter.com/2009/think-globally-tweet-locally) (and later became a 'first-class' Twitter.com user-interface feature). Today, Twitter's APIs drive the two-way communication network that has become the source of breaking news. The opportunities to build on top of this global, real-time communication channel are endless.

Twitter now makes available two historical APIs that provide access to every publicly available Tweet: Historical PowerTrack and the Full-Archive Search API. Both APIs provide a set of *operators* used to query and collect Tweets of interest. These operators match on a variety of attributes associated with every Tweet, hundreds of attributes such as the Tweet's 140-character message, the author's account name, and links shared in the Tweet. Tweets and their attributes are encoded in JSON, a common text-based data-interchange format. So as new features were introduced, new JSON attributes appeared, and typically new API operators were introduced to match on those attributes.  If your use-case includes a need to *listen* to what the world has said on Twitter, the better you understand when operators started having JSON metadata to match on, the more effective your historical PowerTrack filters can be. 

Twitter users organically introduced new, and now fundamental, communication patterns to the Twitter network. A seminal example is the hashtag, now nearly universally used across all social networks. Hashtags were introduced as a way to organize conversations and topics. On a network with hundreds of million messages a day, tools to find Tweets of interest are key, and hashtags have become a fundamental to find Tweets of interest. Soon after the use of hashtags grew, they recieved official status and support from Twitter. As hashtags became a 'first-class' *object*, this meant many things. It meant hashtags became clickable/searchable in the Twitter.com user interface. It also meant hashtags became a member of the Twitter *entities* family, along with @mentions, attached media, stock symbols, and shared links. These entities are conveniently encoded in a pre-parsed JSON array, making it easier for developers to process, scan, and store them. 

Retweets are another example of user-driven conventions becoming official objects. Retweeting emerged as a way of 'forwarding' content to others. It started as a manual process of copying/pasting a Tweet and prepending it with a "RT @" pattern. This process was eventually automated via a new Retweet button, complete with new JSON metadata. The 'official' Retweet was born. Other examples include 'mentions', sharing of media and web links, and sharing a location with your Tweet. Each of these use-patterns resulted in new [twitter.com](https://twitter.com/) user-interface features, new supporting JSON, and thus new ways to match on Tweets. All of these fundamental Tweet attributes have resulted in PowerTrack Operators used to match on them.

So, before we dig into the Historical PowerTrack and Full-Archive Search product details, let's take a tour of how Twitter, as a product and platform, evolved over time. 

### Twitter timeline <a id="=twitterTimeline" class="tall">&nbsp;</a>
Below you will find a select *timeline* of Twitter. Most of these Twitter updates in some way fundamental affected either user behavior, Tweet JSON contents, query Operators, or all three.  Looking at Twitter as a platform, the following events in some way affected the JSON payloads that are used to encode Tweets. In turn, those JSON details affect how Twitter historical API match on them. If you want to dig into those Twitter product details, see our documentation for [Full-Archive Search](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/metadataEvolution/search_timeline.md) and [Historical PowerTrack](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/metadataEvolution/hpt_timeline.md). 

#### 2006
+ October 
    - @replies becomes a convention.  
    - $cashtags first emerge, but using for stock ticker mentions does not become common until early 2009. $Cashtags became a clickable/searchable link in June 2012.
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

#### 2012
+ June - $Cashtags become a clickable/searchable link.

#### 2014
+ March - [Photo tagging and up to four photos supported](https://blog.twitter.com/2014/photos-just-got-more-social). 

#### 2016
+ Febuary - [Searchable GIFs natively hosted in Tweet compose](https://blog.twitter.com/2016/introducing-gif-search-on-twitter). 
+ May - ["Doing More with 140"](https://blog.twitter.com/express-even-more-in-140-characters) (dmw140) first announced. 
+ June - Native video 
+ June - Quoted Retweets generally available. 
+ June - [Stickers introduced for adding to photos](https://blog.twitter.com/2016/introducing-stickers-on-twitter).
+ September - ['Native attachments' introduced](https://twitter.com/Twitter/status/777915304261193728) with trailing URL not counted towards 140 characters ("dwm140, part 1").

#### 2017
+ April - ['Simplified Replies'](https://blog.twitter.com/2017/now-on-twitter-140-characters-for-your-replies) introduced with replied to accounts not counted towards 140 characters ("dmw140, part 2"). 


### Next Steps

Now that we've explored the timeline of when key Twitter features were introduced, the next step is to get into the many details of how these events affected matching on Tweets of interest. These two articles 

+ Historical PowerTrack API: metadata and filtering timeline  
+ Full-Archive Search API: metadata and filtering timeline  


+ Choosing between Historical PowerTrack and Search API
+ Getting Started with Tweet JSON

=======================================
