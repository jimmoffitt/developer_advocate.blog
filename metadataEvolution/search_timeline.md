
## Full-Archive Search API: metadata and filtering timeline

tl;dr

*“As someone using the Full-Archive Search API to access Tweets of interest, I need to understand when query Operators first started matching Tweet JSON attributes."*

--------------------------------------------

+ [Product Overview](#overview)
+ [Matching Metadata Timelines](#metadataTimelines)
+ [Filtering Examples](#filteringExamples)
+ [Next Steps](#nextSteps)

### Product Overview <a id="overview" class="tall">&nbsp;</a>

Full-Archive Search (FAS) was launched in August 2015 and enables customers to immediately access any publicly available Tweet. With FAS you submit a single query and receive a response in classic RESTful fashion. FAS implements 500-Tweets-per-response pagination, and defaults to a 120-requests-per-minute rate-limit. Given these details, FAS can be used to rapidly retrieve Tweets, and at large scale using concurrent requests.

Unlike Historical PowerTrack, whose archive is based on a set of Tweet flat-files on disk, the FAS Tweet archive is much like an on-line database. And as with all databases, it supports making queries on its contents. It also makes use of an *index* to enable high-performance data retrieval. With Full-Archive Search, the querying language is made up of PowerTrack Operators, and these Operators each correspond to a Tweet attribute that is indexed.

Also, unlike HPT, there are Tweet attributes that are updated at the time a query is made (see [HERE](http://support.gnip.com/apis/search_full_archive_api/overview.html#DataUpdates) for more details). For example, if you are accessing a Tweet posted in 2010 today, user details such as Profile bio and location will be updated to today's values and not what they were in 2010. This is true also for Tweet metrics for Favorites and Retweet counts.

### Metadata timelines <a id="metadataTimelines" class="tall">&nbsp;</a>

Below is a timeline of when [Full-Archive Search API Operators](http://support.gnip.com/apis/search_full_archive_api/rules.html#Operators) begin matching. In some cases Operator matching begins well *after* a 'communication convention' becomes commonplace on Twitter. For example, @Replies emerged as a user convention in 2006, but did not become a 'first-class' *object* or *event* with 'supporting' JSON until early 2007. Accordingly, matching on @Replies in 2006 requires an examination of the Tweet body, rather than relying on the ```to``` and ```in_reply_to_status_id``` PowerTrack Operators. 

The details provided here were generated using Full-Archive Search, and were informed by the Twitter timeline provided [HERE](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/metadataEvolution/twtr_evolution.md).  

#### 2006
 + March 26 - ```lang:```
 + July 13 - ```has:mentions```
 + October 6 - ```has:symbols```. $cashtags (or symbols) for discussing stock symbols does not become common until early 2009. Until then most usage was probably slang (e.g., $slang). 
 + October 26 - ```has:links``` 
 + November 23 - ```has:hashtags``` 

#### 2007
 + January 30 - First first-class @reply (in_reply_to_user_id), ```reply_to_status_id:``` begins matching. 
 + August 23 - Hashtag emerge as common convention for organizing topics and conversations. First real use a week later.

#### 2009
+ May 15 - ```is:retweet```. Note that this Operator starts matching with the 'beta' release of official Retweets and its "Via @' pattern. During this beta period, the Tweet verb is 'post' and the original Tweet is not included in the payload.
+ August 13 - Final version of official Retweets is released with "RT @" pattern, a verb set to 'share', and the 'retweet_status' attribute containing the original Tweet (thus approximately doubling the JSON payload size).
+ November 20 - [internal note: non-zero counts, but geo scrubbed at some point] ```has:geo```, ```place_country:```, ```bounding_box:``` and ```point_radius:```

#### 2010
+ March 6 - ```has:geo```, ```bounding_box:``` and ```point_radius:```
+ August 28 - ```has:videos``` (Until February 2015, this Operator matches on Tweets with links to select video hosting sites such as youtube.com, vimeo.com, and vivo.com).

#### 2011
+ July 20 -  ```has:media``` and ```has:images```. Native photos officially announced August 9, 2010.

#### 2014
+ December 3 - (Approximately) *Some* [Enhanced URL metadata](http://support.gnip.com/enrichments/enhanced_urls.html) with HTML title and description begins in payloads. Enhanced metadata more fully emerged in May 2016.

#### 2015
+ February 10 - ```has:videos``` matches on 'native' Twitter videos.
+ February 17 - ```has:profile_geo```, ```profile_country:```, ```profile_region:```, ```profile_locality:```
+ February 17 - ```place_country:``` and ```place:```

#### 2016
+ May 1 - [Enhanced URL metadata](http://support.gnip.com/enrichments/enhanced_urls.html) more fully available, and was officially announced as part of the [Gnip 2.0 launch in August 2016](https://blog.twitter.com/2016/gnip-2-is-here). No associated Operators for these metadata with Search APIs.

#### 2017 
+ February 22 - Poll metadata become available in original format. No associated Operators for these metadata.



### Filtering tips <a id="filteringTips" class="tall">&nbsp;</a>

Given all the above timeline information, it is clear that there are a lot of details to consider when generating historical Tweet datasets. There are two key things to consider:

+ Some metadata have 'born-on' dates so filters can result in *false negatives*. Such searches include Operators reliant on metadata that did not exist for all of part of the search period. For example, if you are searching for Tweets with the ```has:videos``` Operator, you will not have any matches for periods before February 10, 2015. That is because that Operator matches on *native* videos (attached to a Tweet using the Twitter.com user-interface). For a more complete data set of video sharing on Twitter, filters for before Febuary 10, 2015 would need to contain rule clauses that match on URLs common for video hosting.
+ Some metadata has been backfilled with metadata from a time *after* the Tweet was posted. 

As discussed [HERE](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/metadataEvolution/twtr_evolution.md#filtering-tips-identifying-and-filtering-on-tweet-attributes-important-to-your-use-case-) there are several commonly attribute types that commonly focused on when matching on Tweet JSON:
+ Twitter Profiles
+ Original or shared Tweets
+ Tweet language classification
+ Geo-referencing Tweets
+ Shared links media

Some of these have product-specific behavior while others have identical behavior. 

#### Twitter Profiles
    Profile is as it was at the time the Tweet was posted, except for data before 2011. For Tweets older than 2011, the profile metadata reflects the profile as it was in September 2011.

#### Original Tweets and Retweets

[rephrase]
The PowerTrack ```is:retweet``` Operator enables users to filter accordingly. Users of this Operator need to have two strategies for Retweet matching (or not matching) if data before August 2009 is needed. Before August 2009, the Tweet message itself needs to be checked, using exact phrase matching, for matches on “@RT ”. For periods after August 2009, the is:retweet Operator is available.

#### Tweet language classifications  

Twitter’s language classification metadata is available in the archive beginning on March 26, 2013. Note that the Gnip Language classification metadata is in the Activity Stream payload between March 2012. However, with the release of Gnip 2.0 there is no longer an Operator available to match on the Gnip language classification.

#### Geo-referencing Tweets  

As discussed HERE there are three primary ways to geo-reference Tweets:

   + **Geographical references in Tweet message.** Matching on geographic references in the Tweet message, while often the most challenging method since it depends on local knowledge, is an option for the entire Tweet archive. Here is an example geo-referenced match from 2006 for the San Francisco area based on a ‘golden gate’ filter: https://twitter.com/biz/statuses/28311

   + **Tweets geo-tagged by the user.** The available geo-tagging history is dependent on the Historical API you are using. With the Search APIs the ability to start matching on Tweets with some Geo Operators started in March 2010, and with others on February 2015. 
   [] DETAILS HERE

   + **Account profile ‘home’ location set by user.**  As with Tweet geo, the methods to match and the time periods available depends on the Historical API you are using. Profile Geo Operators are available in both Historical PowerTrack and the Search APIs. With the Search APIs, these Profile Geo metadata is available starting in February 2015. 


#### Shared links and media 

In March 2012, the expanded URL enrichment was introduced. Before this time, the Tweet payloads included only the URL as provided by the user. So, if the user included a shortened URL it can be challenging to match on (expanded) URLs of interest. With the Search APIs, these metadata are available starting in March 2012.

In July 2016, the enhanced URL enrichment was introduced. This enhanced version provides a web site’s HTML title and description in the Tweet payload, along with Operators for matching on those. These metadata begin emerging in December 2014.

In September 2016 Twitter introduced 'native attachments' where a trailing shared link is not counted against the 140 Tweet character limit. Both URL enrichments still apply to these shared links.

Here are when related PowerTrack Operators begin matching:
 [] DETAILS HERE
+ has:links
+ has:images and has:media
+ Expanded URLs
+ has:videos
+ Enhanced URL expansion


### Next Steps <a id="nextSteps" class="tall">&nbsp;</a>
+ Historical PowerTrack API: metadata and filtering timeline
+ Choosing between Historical PowerTrack and Search API 
+ Getting Started with Tweet JSON
