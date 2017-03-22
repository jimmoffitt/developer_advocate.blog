# Evolution of Tweet Metadata
+ [Introduction](#introduction)
+ [Twitter JSON Objects 101](#twitterJsonIntro)
+ [Twitter Historical Products](#historicalProducts)

+ [Upcoming articles:](#articleList)
+ [Twitter Platform Timeline](#twitterTimeline)
+ [Choosing an historical API](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/ChoosingHistoricalAPI.md)  
+ [Historical PowerTrack: metadata and filtering timelines](#hptTimeline)
+ [Full-Archive Search API: metadata and filtering timelines](#fasTimeline)

### tl;dr
“As someone doing historical research with Twitter Data, I need to understand how the platform has evolved, how that evolution affected user behavior, how it affected the Tweet JSON payloads, and how to effectively search the Twitter archive. “

## Introduction <a id="introduction" class="tall">&nbsp;</a>

Billions of Tweets have been posted since 2006. These Tweets encapsulate an amazing amount of human communications. This archive can be an indispensable research tool for an incredible number of use-cases. If you are here to learn more about doing historical research with Twitter data, there are three fundamentally different topics to dig into:

 + 1) Evolution of the Twitter Plaform, and the timeline of when new features and enhancements were rolled out. 
 + 2) How Tweets are 'marked up' in JSON and how those encoded attritubes have changed due to new features.
 + 3) How to effectively match those Tweet and User metadata with the PowerTrack querying language that is common to Historical PowerTrack and the Search API. Understanding key differences between these products is key to getting the data you need when you need it. 
 
Understanding the Twitter evolution is helpful since new features map directly to JSON 'objects' and their attributes. Take the Retweet for available. Twitter has always been a place to share information and early in its history users organically started to 'forward', or 'retweet' Tweets they enjoyed and wanted to share. The convention became to prepend the Tweet message with a "RT @user" pattern to indicate original author. Retweeting became so common, in 2009 Twitter built the functionality into a new button and promoted retweeting to a first-class Retweet object. Along with that came new metadata in Tweet JSON payloads: a new verb of 'share', select Retweet metrics, and eventually the entire JSON payload of the original Tweet. With these new JSON attributes in place, along came PowerTrack Operators to help efficiently and effectively match on Retweets. 

These details, which we'll dig into soon, are critical for effective Twitter data filtering. For example, say you are doing research based only on Retweets, and you need to filter out all original Tweets. The easiest way to match on Retweets is to use the PowerTrack ```is:retweet``` Operator. However, that Operator depends on the first-class Retweet metadata that was introduced in April 2009. So if you include a ```is:retweet``` rule clause for before that time, it will not match anything. Instead you would have to build a rule clause that matches on the "RT @" pattern. For more information on how the Twitter platform has evolved see [HERE].

Being familiar with how Tweets are encoded in JSON, and how those encodings changed over the years, is also important. [As in the Retweet example above, many PowerTrack Operators work directly with this JSON metadata. So it's important to know when PowerTrack Operators will result in false-negatives with searching for Tweets. It can be also important to 



### Twitter Historical Products <a id="historicalProducts" class="tall">&nbsp;</a>

Twitter offers two products that provide access to every publicly available Tweet; [Historical PowerTrack](http://support.gnip.com/apis/historical_api2.0/) and [Full-Archive Search](http://support.gnip.com/apis/search_full_archive_api/). These APIs able users to build queries using a set of [PowerTrack Operators](http://support.gnip.com/apis/search_full_archive_api/rules.html#Operators). 

In 2012, Historical PowerTrack (HPT) was introduced and quickly because a widely utilized Twitter research tool. HPT enables users to associate a time period of interest and a set of 1,000 PowerTrack rules/filters to an historical ```Job```. HPT offers the same Operators as real-time PowerTrack and is built to deliver Tweets at scale. In fact, this product is used to generate and share the entire public archive of Tweets to the Library of Congress (LOC). HPT generates a time-series of 10-minute data files for download. These Jobs can result in thousands of large files that take many hours to both generate and download. The HPT API provides a variety of methods to create and monitor a Job's process. Essentially the API is used to manage a Job's lifecycle. 

In 201#, the 30-Day Search API was released. [Product description. ] Next Twitter indexed the entire Tweet archive and in 2015 released Full-Archive Search (FAS). FAS also provides access to the entire Twitter archive, but does it in a much different way. With FAS you submit a single query and receive a response in classic RESTful fashion. FAS implements 500-Tweets-per-response pagination, and defaults to a 120-requests-per-minute rate-limit. Given these details, FAS can be used to rapidly retrieve Tweets, and at large scale using concurrent requests. FAS also provide the ability to count the number of Tweets matching your query before requesting the corresponding data. Counts are avaialable in arrays with minute, hour, and day periods. This ability to 'look before you leap' is an amazing tool in itself. With many use-cases, matching volumes is of primary interest. Since the Counts endpoint provides fast feedback on the matching behavior of a rule, it can be used to assess filtering behavior before pulling the data. For this reason, the Search API is a great complement to real-time and Historical PowerTrack. 





### Next Steps 
What follows is a set of articles that address how these Twitter changes affect the effort to find and analyze Twitter data.

+ Wondering how to decide which historical API to use? --> See [article]
+ Want to learn more about the JSON metadata that defines Tweets? --> [article]
+ Using Full-Archive Search and need to better understand Tweet JSON and filtering timelines? --> [article]
+ Using Historical PowerTrack and need to better understand Tweet JSON and filtering timelines? --> [article]


First we'll a review of Twitter Plaform updates that in some way affected the JSON generated with HPT and FAS. Then we'll dig into the many product-specific details that affect how this stored JSON matches PowerTrack Operators.

=====================================================

# Articles <a id="articleList" class="tall">&nbsp;</a>

## Twitter timeline <a id="twitterTimeline" class="tall">&nbsp;</a>  

User-driven conventions and new features. [Intro narrative on the evolution of hashtags and retweets, and how new twitter features affected user-behavior.]

#### New user conventions and use-patterns driving product development

Hashtags, Repies, Mentions, Retweets, adding links, sharing photos.

#### New features being added to Platform 

Polls, dwm140 p1, p2.  (no need to go into other Platform features, e.g. DMs and Ads)

### Retweets

Retweets have an interesting history on Twitter. 
user convention/uproar --> UI features --> JSON affects --> filtering details. 

### Twitter Entities

Looking at Twitter as a platform, the following events somehow affected the JSON payloads that are used to encode Tweets. This Tweet JSON is a set of Tweet attributes, and these metadata provide the values that PowerTrack Operators match. 

#### 2006
+ October - @replies becomes a convention. 
+ November - Favorites introduced.
#### 2007
+ January - @replies become a first-class object with a UI reply button with ```in_reply_to``` metadata. 
+ April - Retweets become a convention.
+ August #hashtags becomes a tool for searching and organizing Tweets. 
#### 2009
+ February - $cashtags become a convention for discussing stock ticker symbols. 
+ May - Twitter begins making Retweets a first-class object with ```retweet_status``` metadata. 
#### 2010
+ June - Twitter Places introduced for geo-tagging Tweets. 
#### 2016

Other important platform updates:
#### 2010
+ Tweet button launched. This made sharing links easier, so likely increased the number of URLs being shared.
#### 2011
+ Native photos introduced. 
#### 2016
+ dwm140, part 1
#### 2017
+ dmw140, part 2 





## Search API and metadata timeline

## Full-Archive Search API: metadata and filtering timeline  <a id="d=fasTimeline" class="tall">&nbsp;</a>

+ User/Actor object metadata updated at query time. 
 
+ Tweet engagement metadata updated at query time.



### Product timeline


### Metadata JSON timelines

Below are details about when Operator behavior changed. 

#### 2006
 + July 13 - ```has:mentions``` begins matching.
 + October 6 - ```has:symbols``` begins matching (Search). $cashtags for discussing stock symbols does not become common until early 2009.
 + October 26 - ```has:links``` begins matching.
 + November 23 - ```has:hashtags``` begins matching.

#### 2007
 + January 30 - First first-class @reply (in_reply_to_user_id), ```reply_to_status_id:``` begins matching. 
 + August 23 - “hashtag invented” according to internal history. First real use a week later.

#### 2009
+ May 15 - ```is:retweet``` begins matching.  

#### 2010
+ March 6 - ```has:geo``` starts matching. 

### Filtering

+ geo operators
+ User/Actor metadata
+ URL matching

[] is:verified support throughout Search archive. 
[] is:quote not available - strategies for matching in Search?

