## Historical PowerTrack API: metadata and filtering timeline  <a id="hptTimeline" class="tall">&nbsp;</a>

tl;dr

“As someone using Historical PowerTrack to access Tweets of interest, I need to understand when query Operators first started matching Tweet JSON attributes."

### Product overview

Historical PowerTrack (HPT) was launched in July 2012 by Gnip. HPT brought the same filtering capabilites developed for real-time streaming to the entire archive of public Tweets. [] 


### Metadata timelines

Below is a timeline of when Historical PowerTrack Operators begin matching. In some cases Operator matching begins well after a 'communciation convention' becomes common place on Twitter. For example, @Replies emerged as a conventon in 2006, but did not become a 'first-class' object or event with 'supporting' JSON until early 2007. Accordingly, matching on @Replies in 2006 requires an examination of the Tweet body, rather than relying on the ```to``` and ```in_reply_to_status_id``` PowerTrack Operators. 

#### 2007
+ January 3 - ```is:verified``` [this one makes no sense -- [] recheck]
+ January 30 - ```to:``` and ```In_reply_to_status_id:``` @Replies become a first-class event after becoming a user-convention in October 2006.
+ April 1 - ```has:hashtags``` and ```#``` Operator.  Hashtags become a common 'organizing' tool in August.  

#### 2008
+ February 27 - ```has:mentions``` and ```@``` Operator  
+ February 27 - ```has:links``` and ```url:``` [] confirm url

#### 2011
+ January 1 - ```is:retweet``` and ```retweet_of_status_id:``` 
+ June 2 - ```has:images```
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

#### 2017
+ February 22 - Poll metadata is available in *original* format. 

