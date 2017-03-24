
## Twitter Data timeline <a id="twitterTimeline" class="tall">&nbsp;</a>  

tl;dr

"Understanding how Twitter evolved is helpful since new features map directly to JSON 'objects' and their attributes. Queries composed of PowerTrack Operators enable you to find Tweets of interest." 


### Introduction <a id="=introduction" class="tall">&nbsp;</a>

[a fine place to introduce how PowerTrack Operators **match** JSON attributes. ] 

At its core, Twitter is a public, real-time, and global communication network. Since 2006, Twitter's evolution has been driven by both user use-patterns [ or conventions? ] and new product features and enhancements. If you are using Twitter data for historical research, understanding these evolutions is important for building effective queries of the Tweet archive. Twitter makes available two historical APIs that provide access to every publicly available Tweet: Historical PowerTrack and the Full-Archive API. These APIs offer *operators* that are used to identify Tweets of interest. 

Twitter users organically introducted fundamental communication patterns to the Twitter network. A seminal example is the hashtag, now nearly universally used on social networks. Hashtags were introduced as a way to organize conversations and topics. On a network with hundreds of million messages a day, tools to find Tweets of interest are key, and hashtags have become a critical method. 

Retweets are another example. Retweeting emerged as a way of 'forwarding' content to others, started as a manual process of Tweet copy/paste prepended with a "RT @" pattern. This process was eventually automated via a new button, complete with new JSON metadata. The 'official' Retweet was born. Other examples include 'mentions', and sharing of media and web links. Each of these use-patterns resulted in new [twitter.com](https://twitter.com/) user-interface features, new supporting JSON, and thus new ways to match on Tweets. 

Since 2006, Twitter has also evolved as a *Platform*. APIs have always been a pillar of the Twitter network. The first APIs hit the streets soon after Twitter was launched. Today, Twitter's APIs drive the two-way communication network that has become the source of breaking news. The opportunities to build on top of this global, real-time communication channel are endless.  

(no need to go into other Platform features, e.g. DMs and Ads)

[So, what are we covering next?
* We'll start off with more background about the evolution of fundamental Twitter characteristics, starting with Retweets and then collections of Twitter **entities**.

* Then a twitter timeline, along with comments on affects on JSON and query Operators. 
]

### Fundamental Metadata 

#### Retweets

Retweets have an interesting history on Twitter. 

user convention/uproar --> UI features --> JSON affects --> filtering details. 

Beta period --> Official Retweet object with verb, original payload, and select metrics. 

#### Twitter Entities

Hashtags, mentions, urls, media

As these entities became first-class objects they became members of the *entities* collection of Tweet JSON attributes. Providing these entities 


### Twitter timeline
Below you will find a *timeline* of Twitter (as a Product and Platform). Most of these Twitter updates in some way fundamental affected either user behavior, Tweet JSON contents, query Operators, or all three.  Looking at Twitter as a platform, the following events somehow affected the JSON payloads that are used to encode Tweets.

[Included below are comments indicating any JSON and/or filteirng affects?] 
[]

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
+ November - Tweet Geotagging API is launched, providing first method for users to share location via third-party apps. 

#### 2010
+ June - Twitter Places introduced for geo-tagging Tweets. 
+ August - Tweet button for web sites is launched. Made sharing links easier.  

#### 2011
+ M
ay - Follow button introduced, making it easier to follow accounts associated with web sites.  
+ August - Native photos introduced. 

#### 2016
+ June - Quoted Retweets generally available. 
+ September - 'Native attachments' introduced with trailing URL not counted towards 140 characters ("dwm140, part 1").

#### 2017
+ April - 'Simplified Replies' introduced with replied to accounts not counted towards 140 characters ("dmw140, part 2"). 


### Next Steps

[] Choosing between Historical PowerTrack and Search API
[] Historical PowerTrack API: metadata and filtering timeline  
[] Search API: metadata and filtering timeline  

=======================================
