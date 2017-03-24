# Evolution of Tweet Metadata

### tl;dr
“As someone doing historical research with Twitter Data, I need to understand how the platform has evolved, how that evolution affected user behavior, how it affected the Tweet JSON payloads, and how to effectively search the Twitter archive. “

+ [Introduction](#introduction)
+ [Twitter Historical Products](#historicalProducts)
+ [Next Steps](#nextSteps)

Twitter Data timeline: 
+ [Twitter Platform Timeline](#twitterTimeline)

Other drafts in the works:
+ [Twitter JSON Objects 101](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/TweetJSON_intro.md)
+ [Choosing an historical API](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/ChoosingHistoricalAPI.md)  
    + [Related Operator table](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/HistoricalOperatorsTable.md) 
+ [Historical PowerTrack: metadata and filtering timelines](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/hpt_timeline.md)
+ [Full-Archive Search API: metadata and filtering timelines](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/search_timeline.md)

## Introduction <a id="introduction" class="tall">&nbsp;</a>

Billions of Tweets have been posted since 2006. These Tweets encapsulate an amazing amount of human communications and shared information. This archive can be an indispensable research tool for an incredible number of use-cases. [cite examples]

If you are here to learn more about doing historical research with Twitter data, there are three fundamentally different topics to dig into:

 1) Evolution of the Twitter Plaform, and the timeline of when new features and enhancements were rolled out. 
 2) How Tweets are 'marked up' in JSON and how those encoded attritubes have changed due to new features. New JSON attributes often kead to new filtering options.  
 3) How to effectively match those Tweet and User metadata with the PowerTrack querying language that is common to Historical PowerTrack and the Search API. Understanding key differences between these products is key to getting the data you need when you need it. 
 
Understanding the Twitter evolution is helpful since new features map directly to JSON 'objects' and their attributes. Take the Retweet for available. Twitter has always been a place to share information and early in its history users organically started to 'forward', or 'retweet' Tweets they enjoyed and wanted to share. The convention became to prepend the Tweet message with a "RT @user" pattern to indicate original author. Retweeting became so common, in 2009 Twitter built the functionality into a new button and promoted retweeting to a first-class Retweet object. Along with that came new metadata in Tweet JSON payloads: a new verb of 'share', select Retweet metrics, and eventually the entire JSON payload of the original Tweet. With these new JSON attributes in place, along came PowerTrack Operators to help efficiently and effectively match on Retweets. 

These details, which we'll dig into soon, are critical for effective Twitter data filtering. For example, say you are doing research based only on Retweets, and you need to filter out all original Tweets. The easiest way to match on Retweets is to use the PowerTrack ```is:retweet``` Operator. However, that Operator depends on the first-class Retweet metadata that was introduced in April 2009. So if you include a ```is:retweet``` rule clause for before that time, it will not match anything. Instead you would have to build a rule clause that matches on the "RT @" pattern. For more information on how the Twitter platform has evolved see [HERE].

Being familiar with how Tweets are encoded in JSON, and how those encodings changed over the years, is also important. [

As in the Retweet example above, many PowerTrack Operators work directly with this JSON metadata. 

So it's important to know when PowerTrack Operators will result in false-negatives with searching for Tweets. 

]

### Twitter Historical Products <a id="historicalProducts" class="tall">&nbsp;</a>

Twitter offers two products that provide access to every publicly available Tweet; [Historical PowerTrack](http://support.gnip.com/apis/historical_api2.0/) and [Full-Archive Search](http://support.gnip.com/apis/search_full_archive_api/). These APIs able users to build queries using a set of [PowerTrack Operators](http://support.gnip.com/apis/search_full_archive_api/rules.html#Operators). 

In 2012, Historical PowerTrack (HPT) was introduced and quickly because a widely utilized Twitter research tool. HPT enables users to associate a time period of interest and a set of 1,000 PowerTrack rules/filters to an historical ```Job```. HPT offers the same Operators as real-time PowerTrack and is built to deliver Tweets at scale. In fact, this product is used to generate and share the entire public archive of Tweets to the Library of Congress (LOC). HPT generates a time-series of 10-minute data files for download. These Jobs can result in thousands of large files that take many hours to both generate and download. The HPT API provides a variety of methods to create and monitor a Job's process. Essentially the API is used to manage a Job's lifecycle. 

In 2013 (?), the 30-Day Search API was released. [Product description. ] Next Twitter indexed the entire Tweet archive and in 2015 released Full-Archive Search (FAS). FAS also provides access to the entire Twitter archive, but does it in a much different way. With FAS you submit a single query and receive a response in classic RESTful fashion. FAS implements 500-Tweets-per-response pagination, and defaults to a 120-requests-per-minute rate-limit. Given these details, FAS can be used to rapidly retrieve Tweets, and at large scale using concurrent requests. FAS also provide the ability to count the number of Tweets matching your query before requesting the corresponding data. Counts are avaialable in arrays with minute, hour, and day periods. This ability to 'look before you leap' is an amazing tool in itself. With many use-cases, matching volumes is of primary interest. Since the Counts endpoint provides fast feedback on the matching behavior of a rule, it can be used to assess filtering behavior before pulling the data. For this reason, the Search API is a great complement to real-time and Historical PowerTrack. 

### Next Steps  <a id="nextSteps" class="tall">&nbsp;</a>
First we'll a review of Twitter Plaform updates that in some way affected the JSON generated with HPT and FAS. Then we'll dig into the many product-specific details that affect how this stored JSON matches PowerTrack Operators.

What follows is a set of articles that address how these Twitter changes affect the effort to find and analyze Twitter data.

+ Wondering how to decide which historical API to use? --> See [article]
+ Want to learn more about the JSON metadata that defines Tweets? --> [article]
+ Using Full-Archive Search and need to better understand Tweet JSON and filtering timelines? --> [article]
+ Using Historical PowerTrack and need to better understand Tweet JSON and filtering timelines? --> [article]

------------------------

    Standalone but related articles/blog posts/getting-started-guides

--------------------------


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
+ May - Follow button introduced, making it easier to follow accounts associated with web sites.  
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



