---
layout: help_article_nav
category: article
permalink: /articles/hpt-timeline.html
title: Historical PowerTrack API: metadata and filtering timeline   
author: Jim Moffitt
description: When using Twitter's Historical PowerTrack, it is important to understand when PowerTrack Operators first start matching Tweet JSON attributes.
tags: historical twitter rules
path:
- name: Articles
  link: /articles/
heading: Historical PowerTrack Metadata Timeline 
nav:
- name: Introduction
  link: intro
- name: Product Overview
  link: overview
- name: Metadata Timeline
  link: metadataTimeline
- name: Filtering Tips
  link: filteringTips
- name: Next Steps
  link: nextSteps
---

### Introduction  <a id="intro" class="tall">&nbsp;</a>

How Twitter evolved as a platform, and how that affected the JavaScript Object Notation (JSON) used to encode Tweets, is discussed [here](http://support.gnip.com/articles/tweet-timeline.html). That article also begins the discussion of how these JSON details affect creating the filters needed to find your historical data of interest. This article continues that discussion by exploring how these details affect writing filters for Historical PowerTrack. This, and a complementary article about Full-Archive Search, will serve as a 'compare and contrast' discussion of the two Twitter historical products.

### Product Overview <a id="overview" class="tall">&nbsp;</a>

The Historical PowerTrack (HPT) API brings the same filtering capabilities developed for real-time streaming to the entire archive of public Tweets. The HPT API was launched in July 2012 by Gnip, and serves data from an archive first assembled for the HPT launch. HPT makes available every public Tweet ever posted, and was designed to deliver Tweet volumes _at scale_.

The HPT API is used to manage the lifecycle of a historical Job. Using the API, a Job is created with up to 1,000 filtering rules (each one up to 2,048 characters), covering a research period as long as needed. Next a rough estimate of associated Tweets is provided. This estimate is 'order of magnitude' accurate: *is there 100M Tweets associated with my filters, or 100,000*? If the Job is accepted, _every single Tweet_ posted during the period of interest is examined for a match to any included rules. As matching Tweets are found they are written as JSON to a 10-minute time-series of datafiles for download.

With Historical PowerTrack, Tweets are written to the archive as they are posted. However, when the archive was built in 2012 it included Tweet JSON that had been normalized and backfilled to some extent. For example, the "entities" structure that contains hashtags, mentions, and symbols was built out for periods before those entities official existed (see [here](http://support.gnip.com/articles/tweet-timeline.html#keyConcepts) for more background). Furthermore, some metadata, such as whether an account is verified, was backfilled. Accordingly, if you query for early Tweets from 2007, you'll find user profiles that are marked as verified, even though account verification did not begin until 2009.    

As mentioned [in our documentation](http://support.gnip.com/apis/historical_api2.0/overview.html#Caveats), there are important metadata details about the Historical PowerTrack archive: 

+ **URLs**: The url_contains operator will still function prior to 3/26/2012, but will only match against URLs as they are entered by a user into a Tweet and not the fully resolved URL (i.e. if a bit.ly URL is entered in the Tweet it can only match against the bit.ly and not the URL that has been shortened by bit.ly)
+ **Geo**: Native geo data prior to 9/1/2011 is not available in Historical Powertrack. As a result, all operators reliant on this geo data will not be supported for jobs with a timeframe prior to this date.
+ **User Profile Data**: All data prior to 1/1/2011 contains user profile information as it appeared in that user’s profile in September 2011. (e.g @jack’s very first Tweet in March 2006 contains his bio data from September 2011 that references his position as CEO at Square, which was not in existence at the time of the Tweet)
+ **Followers and Friends Counts**: All data prior to 1/1/2011 contains followers and friends counts equal to zero. As a result, any rules based on non-zero counts for these metadata will not return any results for a timeframe prior to this date.
+ **Twitter language** classifications begin appearing in November 2013.
+ **Profile Geo** metadata begins appearing on June 4, 2013.

### Metadata timelines <a id="metadataTimeline" class="tall">&nbsp;</a>

Below is a timeline of when [Historical PowerTrack Operators](http://support.gnip.com/apis/powertrack2.0/rules.html#Operators) begin matching. In some cases Operator matching begins well *after* a 'communication convention' became commonplace on Twitter. For example, @replies emerged as a user convention in 2006, but did not become a ['first-class' *object* or *event* with 'supporting' JSON](http://support.gnip.com/articles/tweet-timeline.html#keyConcepts) until early 2007. Accordingly, matching on @replies in 2006 requires an examination of the Tweet body, rather than relying on the ```to``` and ```in_reply_to_status_id``` PowerTrack Operators. 

The details provided here were generated using HPT (well over 160 HPT Jobs!), and were informed by the Twitter timeline provided [HERE](http://support.gnip.com/articles/tweet-timeline.html). This timeline is not 100% complete or precise. If you identify another filtering/metadata "born on date" fundamental to your use-case, please let us know.  

#### 2007
+ January 3 - ```is:verified``` begins matching.
+ January 30 - ```to:``` and ```in_reply_to_status_id:``` @Replies become a first-class event after becoming a user-convention in October 2006.
+ April 1 - ```has:hashtags``` and ```#``` Operator.  Hashtags become a common 'organizing' tool in August.  

#### 2008
+ February 27 - ```has:mentions``` and ```@``` Operator begin matching.  
+ February 27 - ```has:links``` and ```url:```  begin matching.
+ September - ```emoji``` signal begins appears in HPT estimates.  

#### 2011
+ January 1 - ```is:retweet``` and ```retweet_of_status_id:```. Retweets became a convention as early as April 2007, but matching before this date depends on search for "RT @" or "Via @" patterns.  
+ January 1 - Followers and friends counts begin to be non-zero. ```followers_count:``` and ```friends_count:``` start having non-zero values to match on. 
+ January 1 - User profiles begin to match profiles as they were when Tweet posted. Tweets before this date have their user profiles set to how they existed in September 2011. 
+ June 2 - ```has:images``` and ```has:media``` begin matching.
+ September 1 - ```has:geo```, ```place:```, ```place_country:```, ```bounding_box:``` and ```point_radius:```.

#### 2012
+ March 26 - Gnip introduces new data enrichments. 
    - Gnip Language - ```gnip.lang``` language metadata. No longer filtered for. ```lang:``` Operator now based solely on root level Twitter language classification. 
    - Expanded URLs - URL metadata from this date until launch of HPT 2.0 will contain ```gnip.expanded_url``` fully unwound URL. URL matching before this date will be based on URL as entered by the user. If the Tweet includes a shortened URL, e.g. bit.ly, there will be no expanded URL to match on.
    - Klout Scores - Klout scores from this date until launch of HPT 2.0 will contain ```gnip.klout_score``` data. Klout 2.0 was launched on July 28, 2016.
+ November - ```lang:``` Operator (matching on Twitter language classification).

#### 2013
+ June 4 - ```has:profile_geo```, ```profile_point_radius:```, ```profile_bounding_box:```, ```profile_country:```, ```profile_region:```, ```profile_subregion:```, and ```profile_locality:```.
+ August 20 - ```has:symbols``` and ```$``` Operator. 

#### 2015
+ February 10 - ```has:videos``` begins matching on videos shared through Twitter's user-interface ('native' videos). 
+ September 28 - ```is:quote``` matching on Quoted Tweets. 

#### 2016
+ July 28 - ```url_title:``` and ```url_description:``` supported via [Enchanced URLs enrichment](http://support.gnip.com/enrichments/enhanced_urls.html).
+ July 28 - Klout 2.0 metadata in payloads. No associated Operators for these metadata.

#### 2017
+ January 17 - 'quote_count' and 'reply_count' available in *original* format. No associated Operators for these metadata.
+ February 22 - Poll metadata become available in *original* format. No associated Operators for these metadata.

### Filtering tips <a id="filteringTips" class="tall">&nbsp;</a>

Given all the above timeline information, it is clear that there are a lot of details to consider when generating historical Tweet datasets. There are two key things to consider:

+ Some metadata have 'born-on' dates so filters can result in *false negatives*. Such searches include Operators reliant on metadata that did not exist for all or part of the search period. For example, if you are searching for Tweets with the ```has:videos``` Operator, you will not have any matches for periods before February 10, 2015. That is because that Operator matches on *native* videos (attached to a Tweet using the Twitter user-interface). For a more complete data set of video-sharing Tweets, filters for before Febuary 10, 2015 would need to contain rule clauses that match on common URLs for video hosting.
+ Some metadata, such as user profiles, have been backfilled with metadata from a time *after* the Tweet was posted. 

As discussed [HERE](http://support.gnip.com/articles/tweet-timeline.html#filteringTips) there are several attribute types that are commonly focused on when creating PowerTrack queries:

+ Twitter Profiles
+ Original or shared Tweets
+ Tweet language classification
+ Geo-referencing Tweets
+ Shared links media

Some of these have product-specific behavior while others have identical behavior. See below for more details.

#### Twitter Profiles

With Historical PowerTrack, the profile is as it was at the time the Tweet was posted, *except for data before 2011*. For Tweets older than 2011, the profile metadata reflects the profile as it was in September 2011. 

#### Original Tweets and Retweets

The PowerTrack ```is:retweet``` Operator enables users to either include or exclude Retweets. Users of this Operator need to have two strategies for Retweet matching (or not matching) for data before August 2009. Before August 2009, the Tweet message itself needs to be checked, using exact phrase matching, for matches on the “@RT ” pattern (Actually, if you are filtering on Retweets from between May-August 2009, the "Via @" pattern should be included). For periods after August 2009, the is:retweet Operator is available.

#### Tweet language classifications  

For filtering on a Tweet’s language classification, Twitter’s historical products are quite different. With Historical PowerTrack, Twitter’s language classification metadata is available in the archive beginning in November 2012. Note that the Gnip Language classification metadata is in the Activity Stream payload between March 2012 and launch of PowerTrack 2.0 in 2015. However, with that release there is no longer an Operator available to match on the Gnip language classification.

#### Geo-referencing Tweets  

As discussed [HERE](http://support.gnip.com/articles/geo-intro.html) there are three primary ways to geo-reference Tweets:

   + **Geographical references in Tweet message.** Matching on geographic references in the Tweet message, while often the most challenging method since it depends on local knowledge, is an option for the entire Tweet archive. Here is an example geo-referenced match from 2006 for the San Francisco area based on a ‘golden gate’ filter: https://twitter.com/biz/statuses/28311

   + **Tweets geo-tagged by the user.** The available geo-tagging history is dependent on the Historical API you are using. With Historical PowerTrack, geo-referencing starts on September 1, 2011. When the Historical PowerTrack archive was built, all geo-tagging before this date was not included. 

   + **Account profile ‘home’ location set by user.**  As with Tweet geo, the methods to match and the time periods available depends on the Historical API you are using. With Historical PowerTrack, these Profile Geo metadata is available starting in June 2014. For Tweets posted before the Profile Geo metadata, the ```bio_location:``` Operator is available which can be used to match on non-normalized user input. 
   
#### Shared links and media 

In March 2012, the expanded URL enrichment was introduced. Before this time, the Tweet payloads included only the URL as provided by the user. So, if the user included a shortened URL it can be challenging to match on (expanded) URLs of interest. With Historical PowerTrack  these expanded URL metadata are available starting in March 2012.

In July 2016, the enhanced URL enrichment was introduced. This enhanced version provides a web site’s HTML title and description in the Tweet payload, along with Operators for matching on those. With Historical PowerTrack, these metadata become available in July 2016. 

In September 2016 Twitter introduced 'native attachments' where a trailing shared link is not counted against the 140 Tweet character limit. Both URL enrichments still apply to these shared links.

Here are when related PowerTrack Operators begin matching:

+ 2006 November 1 - ```has:links```
+ 2011 June 2 - ```has:images``` and ```has:media``` for native photos.
+ 2012 March 26 - ```url:``` with the [Expanded URLs enrichment](http://support.gnip.com/enrichments/expanded_urls.html)
+ 2015 February 10 - ```has:videos``` for native videos.
+ 2016 July 28 - ```url_title:``` and ```url_description:``` with the [Enhanced URLs enrichment](http://support.gnip.com/enrichments/enhanced_urls.html)

### Next Steps <a id="nextSteps" class="tall">&nbsp;</a>

+ [Learn more about Historical PowerTrack API](http://support.gnip.com/apis/historical_api2.0/)
+ [Learn more about the evolution of Tweet metadata](http://support.gnip.com/articles/tweet-timeline.html)

**And stayed tuned for these upcoming articles:**

+ Learn more about the Full-Archive Search API and its metadata and filtering timeline
+ Choosing between Historical PowerTrack and Search API

