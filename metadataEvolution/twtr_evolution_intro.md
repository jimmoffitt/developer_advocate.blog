# Evolution of Tweet Metadata

### tl;dr
“As someone doing historical research with Twitter Data, I need to understand how the platform has evolved, how that evolution affected user behavior, how it affected the Tweet JSON payloads, and how to effectively search the Twitter archive. “

+ [Introduction](#introduction)
+ [Twitter Historical Products](#historicalProducts)
+ [Next Steps](#nextSteps)

Other drafts in the works:
+ [Twitter Data Timeline](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/metadataEvolution/twtr_evolution.md)
+ [Historical PowerTrack: metadata and filtering timelines](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/metadataEvolution/hpt_timeline.md)
+ [Full-Archive Search API: metadata and filtering timelines](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/metadataEvolution/search_timeline.md)
+ [Choosing an historical API](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/metadataEvolution/ChoosingHistoricalAPI.md)  
    + [Related Operator table](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/metadataEvolution/HistoricalOperatorsTable.md) 
+ [Twitter JSON Objects 101](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/metadataEvolution/TweetJSON_GettingStarted.md)

## Introduction <a id="introduction" class="tall">&nbsp;</a>

Hundreds of billions of Tweets have been posted since 2006. These Tweets encapsulate an amazing amount of human communications and shared information. This archive can be an indispensable research tool for an incredible number of use-cases. [cite examples]

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

In July 2012, Historical PowerTrack (HPT) was introduced and quickly because a widely utilized Twitter research tool. HPT enables users to associate a time period of interest and a set of 1,000 PowerTrack rules/filters to an historical ```Job```. HPT offers the same Operators as real-time PowerTrack and is built to deliver Tweets at scale. In fact, this product is used to generate and share the entire public archive of Tweets to the Library of Congress (LOC). HPT generates a time-series of 10-minute data files for download. These Jobs can result in thousands of large files that take many hours to both generate and download. The HPT API provides a variety of methods to create and monitor a Job's process. Essentially the API is used to manage a Job's lifecycle. 

In August 2015, the Full-Archive Search (FAS) API was released. Similar in design to the 30-Day Search API, FAS extends access to 
the entire Twitter archive. With FAS you submit a single query and receive a response in classic RESTful fashion. FAS implements 500-Tweets-per-response pagination, and defaults to a 120-requests-per-minute rate-limit. Given these details, FAS can be used to rapidly retrieve Tweets, and at large scale using concurrent requests. 

FAS also provide the ability to count the number of Tweets matching your query before requesting the corresponding data. Counts are avaialable in arrays with minute-by-minute, hourly, and daily intervals. This ability to 'look before you leap' is an amazing tool in itself. With many use-cases, matching volumes is of primary interest. Since the Counts endpoint provides fast feedback on the matching behavior of a rule, it can be used to assess filtering behavior before pulling the data. For this reason, the Search API is a great complement to real-time and Historical PowerTrack. 

### Next Steps  <a id="nextSteps" class="tall">&nbsp;</a>
First we'll a review of Twitter Plaform updates that in some way affected the JSON generated with HPT and FAS. Then we'll dig into the many product-specific details that affect how this stored JSON matches PowerTrack Operators.

What follows is a set of articles that address how these Twitter changes affect the effort to find and analyze Twitter data.

+ Wondering how to decide which historical API to use? --> See [article]
+ Want to learn more about the JSON metadata that defines Tweets? --> [article]
+ Using Full-Archive Search and need to better understand Tweet JSON and filtering timelines? --> [article]
+ Using Historical PowerTrack and need to better understand Tweet JSON and filtering timelines? --> [article]
