# Evolution of the Tweet metadata, and details for effectively querying historical data
+ [Introduction](#introduction)
+ [Twitter Historical Products](#historicalProducts)
+ [Twitter JSON Objects 101](#twitterJsonIntro)
+ [Upcoming articles:](#articleList)
+ [Twitter Platform Timeline](#twitterTimeline)
+ [Choosing an historical API](#selectingHistoricalProduct)  
+ [Historical PowerTrack: metadata and filtering timelines](#hptTimeline)
+ [Full-Archive Search API: metadata and filtering timelines](#fasTimeline)

### tl;dr
“As someone doing historical research with Twitter Data, I need to understand how the platform has evolved, how that evolution affected user behavior, how it affected the Tweet JSON payloads, and how to effectively search the Twitter archive. “

## Introduction <a id="introduction" class="tall">&nbsp;</a>

Billions of Tweets have been posted since 2006. These Tweets encapsulate an amazing amount of human communications. This archive can be an indispensable research tool for an amazing number of use-cases. 

[Set-up the reason the following details are useful]

#### New user conventions and use-patterns driving product development

Hashtags, Repies, Mentions, Retweets, adding links, sharing photos.

#### New features being added to Platform

Private messages, Ads platform, Polls.

### Twitter Historical Products <a id="historicalProducts" class="tall">&nbsp;</a>

Twitter offers two products that provide access to every publicly available Tweet; [Historical PowerTrack](http://support.gnip.com/apis/historical_api2.0/) and [Full-Archive Search](http://support.gnip.com/apis/search_full_archive_api/). These APIs able users to build queries using a set of [PowerTrack Operators](http://support.gnip.com/apis/search_full_archive_api/rules.html#Operators). 

In 2012, Historical PowerTrack (HPT) was introduced and quickly because a widely utilized Twitter research tool. HPT enables users to associate a time period of interest and a set of 1,000 PowerTrack rules/filters to an historical ```Job```. HPT offers the same Operators as real-time PowerTrack and is built to deliver Tweets at scale. In fact, this product is used to generate and share the entire public archive of Tweets to the Library of Congress (LOC). HPT generates a time-series of 10-minute data files for download. These Jobs can result in thousands of large files that take many hours to both generate and download. The HPT API provides a variety of methods to create and monitor a Job's process. Essentially the API is used to manage a Job's lifecycle. 

In 201#, the 30-Day Search API was released. [Product description. ] Next Twitter indexed the entire Tweet archive and in 2015 released Full-Archive Search (FAS). FAS also provides access to the entire Twitter archive, but does it in a much different way. With FAS you submit a single query and receive a response in classic RESTful fashion. FAS implements 500-Tweets-per-response pagination, and defaults to a 120-requests-per-minute rate-limit. Given these details, FAS can be used to rapidly retrieve Tweets, and at large scale using concurrent requests. FAS also provide the ability to count the number of Tweets matching your query before requesting the corresponding data. Counts are avaialable in arrays with minute, hour, and day periods. This ability to 'look before you leap' is an amazing tool in itself. With many use-cases, matching volumes is of primary interest. Since the Counts endpoint provides fast feedback on the matching behavior of a rule, it can be used to assess filtering behavior before pulling the data. For this reason, the Search API is a great complement to real-time and Historical PowerTrack. 


### Twitter JSON Objects 101 <a id="twitterJsonIntro" class="tall">&nbsp;</a>

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


# Articles <a id="articleList" class="tall">&nbsp;</a>

## Twitter timeline <a id="twitterTimeline" class="tall">&nbsp;</a>  

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





## Choosing an historical API <a id="selectingHistoricalProduct" class="tall">&nbsp;</a>  

### Overview

Both Historical PowerTrack (HPT) and Full-Archive Search (FAS) can serve any publicly available Tweet from the entire archive, starting with the first Tweet from March 2006. To better understand ideal uses for Full-Archive Search versus those for Historical PowerTrack, it is important to have a clear understanding of the underlying technologies.

Full-Archive Search is most analogous to Google, obviously another search-based tool. When you search for a particular term, Google doesn’t return or display the millions of matching results all at once. Rather, it delivers 10 results at a time per scrollable page and allows you to click “Next” for the subsequent page of 10 results… and so on, through all of the matching items returned. Like Google, the Full-Archive Search tool is best-suited for delivering smaller datasets at a time with the ability to paginate for more data as needed. It is also the best solution for those situations where expediency in returns is needed. Thanks to our having indexed every Tweet since the beginning of time, it offers near-immediate responses to these smaller data queries.

FAS is designed using the classic request/response pattern, where a single PowerTrack rule is submitted and a response with matching Tweets is immediately provided. FAS can provide a maximum of 500 Tweets per response, and a ‘next’ token is provided to paginate until all Tweets  for a query are received. FAS also supports ‘count’ requests, where only the number of matching Tweets is provided, These counts are returned in a time-series of minute-by-minute, hourly, or daily totals.

Historical PowerTrack, on the other hand, can be compared to a library with copies of all the books that have been published in human existence. If you want to find every mention of a particular keyword or rule, you need to search through every page of every book to find them.  You may know the section of the library, the aisle, the shelf or even the specific book, but you still have to skim every page to find the exact matches of the search. 

HPT is built to deliver Tweets at scale using a batch, Job-based design where the API is used to move a HPT Job through multiple phases. These phases include volume estimation, Job acceptance/rejection, getting Job status, and downloading potentially many thousands of data files. Depending on the length of the request time period, Jobs can take hours or days to generate.


### Fundamental Differences

Here are the fundamental differences between Historical PowerTrack (HPT) and Full-Archive Search (FAS):

+ Number of rules supported per request.
    + Full-Archive Search accepts a single rule per request. Each rule can be up to 2,048 characters. A Historical PowerTrack Job can support up to 1,000 rules. 
+ API Data Response.
     + HPT generates a time-series of data files, each covering a ten-minute period. For example, each hour of data is provided in six 10-minute data files. Inside each HPT file, the JSON Tweet payloads are written in an atomic fashion, and are not presented in an JSON array. File contents need to be parsed using newline characters as a delimiter.
     + With FAS, Tweets in each response are arranged in a “results” array. A maximum of 500 Tweets are available per response and a ‘next’ token is provided if more Tweets are available. For example, if a 60-day request for a single PowerTrack rule matches 10,000 Tweets, at least 20 requests must be made of the Search API.
+ Supported PowerTrack Operators.
    + While the majority of Operators supported by HPT are also supported by FAS, there are a set of Operators not available in FAS:
    + For a complete list of Operators for each product see here:
        + [Search Operator List](http://support.gnip.com/apis/search_full_archive_api/rules.html#Operators)
        + [Historical PowerTrack Operator List](http://support.gnip.com/apis/powertrack2.0/rules.html#Operators)
        
<table class="tg">
  <tr>
    <th class="tg-nhkk">Operator</th>
    <th class="tg-nhkk">PowerTrack (real-time and Historical)</th>
    <th class="tg-nhkk">Search APIs (30-Day and Full-Archive)</th>
  </tr>
  <tr>
    <td class="tg-yw4l">"exact phrase match"</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">@</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-f3f0">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">#</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">$</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">bio:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">bio_location:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">bio_name:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">bounding_box:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">contains:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">&lt;emoji&gt;</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">followers_count:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">friends_count:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">from:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">has:geo</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">has:hashtags</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">has:images</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">has:lang</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">has:links</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">has:media</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">has:mentions</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">has:profile_geo</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">has:symbols</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">has:videos</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">in_reply_to_status_id:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">is:quote</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">is:retweet</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">is:verified</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">keyword</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">lang:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">listed_count:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">place_country:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">place:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">point_radius:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">profile_bounding_box</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">profile_country:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">profile_locality:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">profile_point_radius</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">profile_region:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">profile_subregion:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">proximity~N</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">retweets_of_status_id:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">retweets_of:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">sample:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-3we0"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">source:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">statuses_count:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">time_zone:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">to:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
  <tr>
    <td class="tg-yw4l">url_contains:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">url_description:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">url_title:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">url:</td>
    <td class="tg-4qcj">✔</td>
    <td class="tg-4qcj">✔</td>
  </tr>
</table>

### So what does that mean?
Put another way, Full-Archive Search is data-constrained, while Historical PowerTrack is time or work-constrained. Using Full-Archive Search, the more data that matches your query the longer it will take you to retrieve all of that data through the product. This is because you will need to stitch together the paginated groups of results, one after the other, to create the complete file of all returned data. Using Historical PowerTrack, the more data you are asking to process or “amount of hay in the haystack”, the longer it will take to retrieve all of the data you are looking for, regardless of the number of needles in said haystack. We still have to look at every straw of hay to decide whether it is indeed hay or a needle described in the query. Think of the hay as the Twitter Firehose and the volume of hay that needs to be searched through is akin to the number of historical days requested.

### How do I choose one over the other?
A general assumption to be made here is that Full-Archive Search is better-suited for lower-volume jobs, while Historical PowerTrack is more appropriate for higher-volume jobs and use cases. We’ve intentionally left those descriptions relatively vague, though, as there is no real technical reason why Full-Archive Search could not be used for large data requests. It is just not necessarily practical or efficient, so HPT should be your first choice.
 
### Is there a technical decision point or threshold event?
Depending on how you plan to retrieve the data and utilize rate limits, there actually is a threshold where Historical PowerTrack actually processes the data faster that Full-Archive Search. HPT typically processes a 30-day duration job in about 4 hours, regardless of the volume of matching data returns. To pull 3.5M Tweets via Full-Archive Search, it would require a minimum of 7000 search requests. If you also assume 2 second response times and the need to serially paginate through the data, it would actually take Full-Archive Search longer to pull all of the Tweets.
Keep in mind that Full-Archive Search responses are not capped at a maximum of 500 results  simply “to charge more for the product”. Instead, it is because the technology and infrastructure that the Search APIs are built upon aren’t designed to instantly deliver large volumes of data. There are optimizations you can make to retrieve the data faster via Full-Archive Search through parallel requests, but we can discuss those separately.
Historical PowerTrack is also better suited for large query rulesets, as the Search API products only support a single PowerTrack rule per request. HPT, on the other hand, supports up to one thousand (1,000) rules. Historical PowerTrack is also the right product if you require certain operators that aren’t currently supported in Full-Archive Search. The list of these operators can be found HERE. While we hope to close this functionality gap in time, some operators just aren’t a good fit for our Search API technology and the usage/adoption of others simply don’t justify the cost of indexing them.

### Are there other reasons to choose HPT over Search?
Historical PowerTrack is also a better solution for large, complex rules sets and/or use of certain filters.  Historical PowerTrack supports up to 1000 complex rules per job whereas Search is limited to a single rule per request.  That means to pull data for 1000 rules via search for 6 months, I'd have to send a minimum of 6000 requests to the Full-Archive Search API as opposed to a single HPT job.  Doing so becomes complex, costly and time consuming.  There are also a number of operators that are supported by Historical PowerTrack that are not currently or may never be supported by our search products.  While we aspire to have operator parity across the products and will work to close the gap that currently exists, that ultimate goal may never actually come to fruition.  Some operators supported by PowerTrack and Historical PowerTrack are extremely expensive or difficult to support in a search product, they simply aren't well suited for the underlying Search technologies.  Operator support is also much more expensive for search than it is for PowerTrack and HPT, and in some cases customer usage levels and the associated revenue do not justify the cost of supporting them in Search.

### Switching between products

#### Steps for converting a HPT Job to Full-Archive Search:

Integrate the Full-Archive Search API.
FAS product documentation is available HERE.
FAS API documentation is available HERE.
If already using 30-Day Search, the same code used for 30-Day Search can be used for Full-Archive Search.
Example client code in Python and Ruby are available HERE.
Review HPT rules and confirm all Operators are supported by Search API.
Multiple rules associated with a HPT Job need to be split up into individual Search requests. Each rule will be individually requested from the FAS API.
When receiving API responses:
Tweets are returned in a “results” array, starting with most recent Tweets.
A maximum of 500 Tweets are provided per response.
A ‘next’ token is provided if more requests are needed to complete request. The ‘next’ token should be parsed from the response and added to the next API request.

#### Steps for converting FAS requests to a HPT Job:




## Historical API: metadata and filtering timeline  <a id="hptTimeline" class="tall">&nbsp;</a>

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

