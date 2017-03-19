## Introduction

Billions of Tweets have been posted since 2006. These Tweets encapsulate an amazing amount of human communications. This archive can be an indispensable research tool for an amazing number of use-cases. 

[Set-up the reason the following details are useful]

#### New user conventions and use-patterns driving product development

Hashtags, Repies, Mentions, Retweets, adding links, sharing photos.


#### New features being added to Platform

Private messages, Ads platform, Polls.



### Twitter Historical Products

Twitter offers two products that provide access to every publicly available Tweet. These APIs able users to build queries using a set of [PowerTrack Operators](http://support.gnip.com/apis/search_full_archive_api/rules.html#Operators). 

In 2012, Historical PowerTrack (HPT) was introduced and quickly because a widely utilized Twitter research tool. HPT enables users to associate a time period of interest and a set of 1,000 PowerTrack rules/filters to an historical ```Job```. HPT offers the same Operators as real-time PowerTrack and is built to deliver Tweets at scale. In fact, this product is used to generate and share the entire public archive of Tweets to the Library of Congress (LOC). HPT generates a time-series of 10-minute data files for download. These Jobs can result in thousands of large files that take many hours to both generate and download. The HPT API provides a variety of methods to create and monitor a Job's process. Essentially the API is used to manage a Job's lifecycle. 

In 201#, the 30-Day Search API was released. [Product description. ] Next Twitter indexed the entire Tweet archive and in 2015 released Full-Archive Search (FAS). FAS also provides access to the entire Twitter archive, but does it in a much different way. With FAS you submit a single query and receive a response in classic RESTful fashion. FAS implements 500-Tweets-per-response pagination, and defaults to a 120-requests-per-minute rate-limit. Given these details, FAS can be used to rapidly retrieve Tweets, and at large scale using concurrent requests. FAS also provide the ability to count the number of Tweets matching your query before requesting the corresponding data. Counts are avaialable in arrays with minute, hour, and day periods. This ability to 'look before you leap' is an amazing tool in itself. With many use-cases, matching volumes is of primary interest. Since the Counts endpoint provides fast feedback on the matching behavior of a rule, it can be used to assess filtering behavior before pulling the data. For this reason, the Search API is a great complement to real-time and Historical PowerTrack. 


### Twitter JSON objects 101

Tweets are made up of a Tweet message, a posted time, a set of User (or Author or Actor) attributes, a collection of engagement metadata, and sometimes geographical metadata.

+ Tweet body
+ User object
+ Twitter entities
+ Data enrichments
+ Geo 


### Next Steps 
What follows is a set of articles that address how these Twitter changes affect the effort to find and analyze Twitter data.

[Intro narrative on the evolution of hashtags and retweets, and how new twitter features affected user-behavior.]

We'll start with a review of Twitter Plaform updates that in some way affected the JSON generated with HPT and FAS. Then we'll dig into the many product-specific details that affect how this stored JSON matches PowerTrack Operators. At the architectural level, the HPT and FAS archives are significantly different. [two slightly different list of available PowerTrack Operators.] 


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


## Historical API and metadata timeline

### Product timeline

### Metadata timelines
#### 2007
+ January 3 - is:verified begins matching.
#### 2008
+ February 27 - ```has:mentions``` and ```has:links``` begins matching.
#### 2011
+ September 1 - Tweet Geo starts. Matching for *has:geo, place_country:, bounding_box: point_radius:*.
#### 2012
+ March 26 
 - Gnip Language - ```gnip.lang``` language metadata. No longer filtered for. ```lang:``` Operator now based solely on root level Twitter language classification. 
 - Expanded URLs - URL metadata from this date until launch of HPT 2.0 will contain ```gnip.expanded_url``` fully unwound URL. 
 - Klout Scores - Klout scores from this date until launch of HPT 2.0 will contain ```gnip.klout_score``` data.
#### 2013
+ March 26 - Twitter language classifiction added. 
+ June 4 - Profile Geo launched.
#### 2015
+ September 28 - is:quote begins matching. 
#### 2017
+ February 22 - Poll metadata is available in *original* format. 
