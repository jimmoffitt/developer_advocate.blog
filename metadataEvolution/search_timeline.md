
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

Unlike Historical PowerTrack, whose archive is based on a set of Tweet flat-files on disk, the FAS Tweet archive is much like an on-line database. And as with all databases, it supports making queries on its contents. It also enables the maintenance of an *index* to enable high-performance data retrieval. With Full-Archive Search, the querying language is made up of PowerTrack Operators, and these Operators each correspond to a Tweet attribute that is indexed.

Also, unlike HPT, there are Tweet attributes that are updated at the time a query is made (see [HERE](http://support.gnip.com/apis/search_full_archive_api/overview.html#DataUpdates) for more details). For example, if you are accessing a Tweet posted in 2010 today, user details such as Profile bio and location will be updated to today's values and not what they were in 2010. This is true also for Tweet metrics for Favorites and Retweet counts.

### Metadata timelines <a id="metadataTimelines" class="tall">&nbsp;</a>

Below is a timeline of when Search API PowerTrack Operators begin matching. In some cases Operator matching begins well *after* a 'communication convention' becomes commonplace on Twitter. For example, @Replies emerged as a user convention in 2006, but did not become a 'first-class' *object* or *event* with 'supporting' JSON until early 2007. Accordingly, matching on @Replies in 2006 requires an examination of the Tweet body, rather than relying on the ```to``` and ```in_reply_to_status_id``` PowerTrack Operators. 

The details provided here were generated using Full-Archive Search, and were informed by the Twitter timeline provided [HERE](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/metadataEvolution/twtr_evolution.md).  

#### 2006
 + March 26 - ```lang:```
 + July 13 - ```has:mentions```
 + October 6 - ```has:symbols```. $cashtags for discussing stock symbols does not become common until early 2009.
 + October 26 - ```has:links``` 
 + November 23 - ```has:hashtags``` 

#### 2007
 + January 30 - First first-class @reply (in_reply_to_user_id), ```reply_to_status_id:``` begins matching. 
 + August 23 - “hashtag invented” according to internal history. First real use a week later.

#### 2009
+ May 15 - ```is:retweet```. Note that this Operator starts matching with the 'beta' release of official Retweets and its "Via @' pattern. During this beta period, the Tweet verb is 'post' and the original Tweet is not included in the payload.
+ August 13 - Final version of official Retweets is released with "RT @" pattern, a verb set to 'share', and the 'retweet_status' attribute containing the original Tweet (thus approximately doubling the JSON payload size).
+ November 20 - ```has:geo```, ```place_country:```, ```bounding_box:``` and ```point_radius:```

#### 2010
+ March 6 - ```has:geo```  
+ August 28 - ```has:videos``` (Until 2015, this Operator matches on Tweets with links to select video hosting sites such as youtube.com, vimeo.com, and vivo.com).

#### 2011
+ July 20 -  ```has:media``` and ```has:images```. Native photos officially announced August 9, 2010.

#### 2015
+ February 17 - ```has:profile_geo```, ```profile_country:```, ```profile_region:```, ```profile_locality:```
+ February 17 - ```place_country:``` and ```place:``` 

### Filtering Tips

```
  [] To-do: Needed? More narratives around "OK, so what?" Maybe a FAQ structure? Or wrap into timeline above?
```
+ User/Actor metadata

+ URL matching

+ geo operators
  + Profile geo: Metadata is in payload Tweets starting when it was introducted. However, the Search index on this metadata goes back until 2015-02-17. For example, searches for before this date with the ```profile_country:us``` rule clause will not match anything.
  + Any Tweet-geo search for before March 6, 2010 will have no matches. 
    

### Next Steps <a id="nextSteps" class="tall">&nbsp;</a>
+ Historical PowerTrack API: metadata and filtering timeline
+ Choosing between Historical PowerTrack and Search API 
+ Getting Started with Tweet JSON
