## Historical PowerTrack API: metadata and filtering timeline  <a id="hptTimeline" class="tall">&nbsp;</a>

tl;dr

*“As someone using Historical PowerTrack to access Tweets of interest, I need to understand when query Operators first started matching Tweet JSON attributes."*

--------------------------------------------

+ [Product Overview](#overview)
+ [Matching Metadata Timelines](#metadataTimelines)
+ [Filtering Examples](#filteringExamples)
+ [Next Steps](#nextSteps)


### Product Overview <a id="overview" class="tall">&nbsp;</a>

Historical PowerTrack (HPT) was launched in July 2012 by Gnip. HPT brings the same filtering capabilites developed for real-time streaming to the entire archive of public Tweets. [Built on the first Tweet archive, made up of flat-files.  Built to deliver Tweet volumes at scale. HPT API is used to manage the life-cycle of a Job. A Job is first created with up to 1000 filtering rules, covering a time period as long as needed. Next a rough estimate of associated Tweets is provided. If the Job is accepted, every single Tweet posted during the period of interest is examined for a match to any included rules, and the matched Tweets are written to a 10-minute time-series of data file for download.

With Historical PowerTrack, Tweets are written to the archive as they are posted. However, when the archive was built in 2012 the Tweet JSON was normalized and backfilled to some extent. For example, the "entities" structure that contains hashtags, mentions, and symbols was built out for periods before those entities existed. Furthermore, some metadata, such as whether an account is verified, was backfilled. Accordingly, if you query for early Tweets from 2007, you'll find user profiles that are marked as verified, even though account verification did not begin until 2009.    

As mentioned [HERE](http://support.gnip.com/apis/historical_api2.0/overview.html#Caveats), here are important metadata details of the Historical PowerTrack archive: 

+ **URLs**: The url_contains operator will still function prior to 3/26/2012, but will only match against URLs as they are entered by a user into a Tweet and not the fully resolved URL (i.e. if a bit.ly URL is entered in the Tweet it can only match against the bit.ly and not the URL that has been shortened by bit.ly)
+ **Geo**: Native geo data prior to 9/1/2011 is not available in Historical Powertrack. As a result, all operators reliant on this geo data will not be supported for jobs with a timeframe prior to this date.
+ **User Profile Data**: All data prior to 1/1/2011 contains user profile information as it appeared in that user’s profile in September 2011. (e.g @jack’s very first Tweet in March 2006 contains his bio data from September 2011 that references his position as CEO at Square, which was not in existence at the time of the Tweet)
+ **Followers and Friends Counts**: All data prior to 1/1/2011 contains followers and friends counts equal to zero. As a result, any rules based on non-zero counts for these metadata will not return any results for a timeframe prior to this date.

[Contradictions --> doc updates? others?]
+ Docs say Twitter lang starts on March 26, 2013, testing shows it starts November 2012.
+ Docs say Profile Geo starts August 1, 2013, testing shows it starts June 4, 2013.


### Metadata timelines <a id="metadataTimelines" class="tall">&nbsp;</a>

Below is a timeline of when Historical PowerTrack Operators begin matching. In some cases Operator matching begins well after a 'communciation convention' becomes common place on Twitter. For example, @Replies emerged as a conventon in 2006, but did not become a 'first-class' object or event with 'supporting' JSON until early 2007. Accordingly, matching on @Replies in 2006 requires an examination of the Tweet body, rather than relying on the ```to``` and ```in_reply_to_status_id``` PowerTrack Operators. 

The details provided here were generated using HPT, and were informed by the Twitter timeline provided [HERE](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/metadataEvolution/twtr_evolution.md).  

#### 2007
+ January 3 - ```is:verified``` 
+ January 30 - ```to:``` and ```In_reply_to_status_id:``` @Replies become a first-class event after becoming a user-convention in October 2006.
+ April 1 - ```has:hashtags``` and ```#``` Operator.  Hashtags become a common 'organizing' tool in August.  

#### 2008
+ February 27 - ```has:mentions``` and ```@``` Operator  
+ February 27 - ```has:links``` and ```url:``` [] confirm url

#### 2011
+ January 1 - ```is:retweet``` and ```retweet_of_status_id:```. Retweets became a convention as early as April 2007, but matching before this date depends on search for "RT @" or "Via @" patterns.  
+ June 2 - ```has:images``` and ```has:media```
+ September 1 - ```has:geo```, ```place_country:```, ```bounding_box:``` and ```point_radius:```

#### 2012
+ March 26 - Gnip introduces new data enrichments. 
    - Gnip Language - ```gnip.lang``` language metadata. No longer filtered for. ```lang:``` Operator now based solely on root level Twitter language classification. 
    - Expanded URLs - URL metadata from this date until launch of HPT 2.0 will contain ```gnip.expanded_url``` fully unwound URL. 
    - Klout Scores - Klout scores from this date until launch of HPT 2.0 will contain ```gnip.klout_score``` data.
+ November - ```lang:``` Operator (matching on Twitter language classification).

#### 2013
+ June 4 - ```has:profile_geo``` 
+ August 20 - ```has:symbols``` and ```$``` Operator 

#### 2015
+ February 10 - ```has:videos```  
+ September 28 - ```is:quote```

#### 2016
+ July 28 - ```url_title:``` and ```url_description:```
+ July 28 - Klout 2.0 metadata in payloads. No Operators match on metadata.

#### 2017
+ January 17 - 'quote_count' and 'reply_count' available in *original* format. No Operators match on metadata.
+ February 22 - Poll metadata become available in *original* format. No Operators match on metadata.

### Filtering tips <a id="filteringExamples" class="tall">&nbsp;</a>

[] To-do: Needed?

+ Retweet example
+ Geo example
+ URL example
+ Others

### Next Steps <a id="nextSteps" class="tall">&nbsp;</a>
+ Search API: metadata and filtering timeline
+ Choosing between Historical PowerTrack and Search API 
