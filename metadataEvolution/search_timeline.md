
## Full-Archive Search API: metadata and filtering timeline

tl;dr

*“As someone using the Full-Archive Search API to access Tweets of interest, I need to understand when query Operators first started matching Tweet JSON attributes."*

--------------------------------------------

+ [Product Overview](#overview)
+ [Matching Metadata Timelines](#metadataTimelines)
+ [Filtering Examples](#filteringExamples)
+ [Next Steps](#nextSteps)

### Product Overview <a id="overview" class="tall">&nbsp;</a>

+ User/Actor object metadata updated at query time. 
 
+ Tweet engagement metadata updated at query time.

### Metadata timelines <a id="metadataTimelines" class="tall">&nbsp;</a>

Below is a timeline of when Search API PowerTrack Operators begin matching. In some cases Operator matching begins well after a 'communciation convention' becomes common place on Twitter. For example, @Replies emerged as a conventon in 2006, but did not become a 'first-class' object or event with 'supporting' JSON until early 2007. Accordingly, matching on @Replies in 2006 requires an examination of the Tweet body, rather than relying on the ```to``` and ```in_reply_to_status_id``` PowerTrack Operators. 

The details provided here were generated using Full-Archive Search, and were informed by the Twitter timeline provided [HERE](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/metadataEvolution/twtr_evolution.md).  

#### 2006
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
+ June 2 - ```has:media``` and ```has:images```. Native photos officially announced August 9.
+ August 28 - ```has:videos``` (Until 2015, this Operator matches on #video).

#### 2011
+ July 20 - ```has:images``` 

#### 2015
+ February 17 - ```has:profile_geo```, ```profile_country:```, ```profile_region:```, ```profile_locality:```
+ February 17 - ```place_country:``` and ```place:``` 

### Filtering

+ geo operators
+ User/Actor metadata
+ URL matching

[] is:verified support throughout Search archive. 
[] is:quote not available - strategies for matching in Search?
