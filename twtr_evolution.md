# Introduction

Billions of Tweets have been posted since 2006. These Tweets encapsulate an amazing amount of human communications. This archive is an amazing research dataset that has supported an amazing number of use-cases. [Set-up the reason the following details are useful]

Gnip offers two sets of APIs that provide access to every publicly available Tweet. These products able users to build queries using a set of [PowerTrack Operators](http://support.gnip.com/apis/search_full_archive_api/rules.html#Operators). 

In 2012, Historical PowerTrack (HPT) was introduced and quickly because a widely utilized Twitter research tool. HPT enables users to associate a time period of interest and a set of 1,000 PowerTrack rules/filters to an historical ```Job```. HPT offers the same Operators as real-time PowerTrack and is built to deliver Tweets at scale. In fact, this product is used to generate and share the entire public archive of Tweets to the Library of Congress (LOC). HPT generates a time-series of 10-minute data files for download. These Jobs can result in thousands of large files that take many hours to both generate and download. The HPT API provides a variety of methods to create and monitor a Job's process. Essentially the API is used to manage a Job's lifecycle. 

In 201#, the 30-Day Search API was released. 


What follows is a set of articles that address how these Twitter changes affected the effort to find and analyze Twitter data.

We'll start with a review of Twitter Plaform updates that somehow affected the JSON generated with HPT and FAS. Then we'll dig into the many product-specific details that affect how this stored JSON matches PowerTrack Operators. At the architectural level, the HPT and FAS archives are significantly different. [two slightly different list of available PowerTrack Operators.] 



# Articles

## Twitter timeline

User-driven conventions and new features.

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





## Choosing an historical API 

## Search API and metadata timeline

+ User/Actor object metadata updated at query time. 
 
+ Tweet engagement metadata updated at query time.



### Product timeline


### Metadata JSON timelines

Below are details about when Operator behavior changed. 

#### 2006
 + July 13 - has:mentions begins matching
 + October 6 - has:symbols begins matching (Search). $cashtags for discussing stock symbols does not become common until early 2009.
 + October 26 - has:links begins matching
 + November 23 - has:hashtags begins matching

#### 2007
 + January 30 - First first-class @reply (in_reply_to_user_id)
 + April 18 - While not a first-class event yet, retweets become a thing. 
 + August 23 - “hashtag invented” according to internal history. First real use a week later.



### Filtering

+ geo operators

+ 

[] is:verified support throughout Search archive. 
[] is:quote not available - strategies for matching in Search?


## Historical API and metadata timeline

### Product timeline

### Metadata timelines

#### 2007
+ January 3 - is:verified begins matching.
#### 2011
+ September 1 - Tweet Geo starts. Matching for *has:geo, place_country:, bounding_box: point_radius:*.
#### 2012
+ March 26 
 + Gnip Language - ```gnip.lang``` language metadata. No longer filtered for. ```lang:``` Operator now based solely on root level Twitter language classification. 
 + Expanded URLs - URL metadata from this date until launch of HPT 2.0 will contain ```gnip.expanded_url``` fully unwound URL. 
 + Klout Scores - Klout scores from this date until launch of HPT 2.0 will contain ```gnip.klout_score``` data.
#### 2013
+ March 26 - Twitter language classifiction added. 
+ June 4 - Profile Geo launched.
#### 2015
+ September 28 - is:quote begins matching. 
#### 2017
+ February 22 - Poll metadata is available in *original* format. 
