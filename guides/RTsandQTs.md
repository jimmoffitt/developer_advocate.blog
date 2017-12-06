
[Getting major overhaul, to have narrative/examples change from AS to original format. Correcting/updating Quote Tweet content. Adding in relevant b140 details]

Example Tweets:
https://twitter.com/FloodSocial/status/928950635860545537

Simple quote Tweet
https://twitter.com/SnowBotDev/status/925390480744923136

Extended quote Tweet x 2
https://twitter.com/FloodSocial/status/936771032077107200

+ [Introduction](#intro)
+ [How to match on Retweets and Quote Tweets](#match)
+ [Parsing Retweets and Quote Tweets JSON](#parse)
+ [Next Steps](#next)

# Introduction <a id="intro" class="tall">&nbsp;</a>

Twitter customers often want to know the specifics around identifying and integrating Retweets and Quote Tweets into their products, but can run into a few common roadblocks. If you’re looking for the best way to incorporate Retweets and Quote Tweets into your product, this guide will provide everything you need to know about identifying them, and best practices for extracting the information you need from them. 

Let's start off with some fundamental descriptions.

## What is a Retweet?

A Retweet is an action taken by a Twitter user to share another user’s Tweet without alteration, using Twitter’s explicit Retweet functionality. 

https://twitter.com/Arapahoe_Basin/status/928290029436248064

A Retweet retains information about the user who posted the original Tweet, as well as the user who Retweeted them. Retweets are an important part of Twitter’s platform – they permit content to be shared rapidly and with attribution, and are the most easily measured form of content engagement on the platform. Many social analytics tools use the number of Retweets a particular Tweet receives in calculating its impact or reach (i.e. its importance). However, to do so, your app must be able to accurately identify Retweets.

## What is a Quoted Tweet?

Quote Tweets are another way of sharing Tweets that includes adding your own new content as a comment. Quote Tweets can be selected after using Twitter’s Retweet option. 

https://twitter.com/SnowBotDev/status/925390480744923136

In some ways Quote Tweets can be thought of as a special kind of Retweet. They retain information about the user who posted the Tweet being quoted, as well as the user who Quoted them. 

{Need to discuss retweet and quote counts and describe which ones include both...}


# How to match on Retweets and Quote Tweets <a id="match" class="tall">&nbsp;</a>

## Matching on Retweets

+ ```is:retweet```:
+ ```retweets_of:```
+ ```retweets_of_status_id:```


## Matching Quote Tweets

+ ```is:quote```:

{what other keywords match on quote text}



# Parsing Retweet and Quote Tweet JSON <a id="parse" class="tall">&nbsp;</a>

Three types of Tweets are involved: original, Retweet and Quote Tweet.



{This discussion is based on the *native* Tweet JSON format.}

Two forms of Tweet JSON are available. This content will focus on the Twitter "native" (or "original") format. See HERE for the first version of this content, written for the Activity Streams format.

### Many possible combinations

2 cases:
Retweet - Original Tweet 
Retweet - Original *extended* Tweet

Many cases: 
Quote - Original Tweet
*Extended* Quote - Original Tweet

Quote - Original *extended* Tweet
*Extended* Quote - Original *extended* Tweet

TEST:
Quote - Quote 
*Extended* Quote - Quote 

Quote - *extended* Quote 
*Extended* Quote - *extended* Quote


### Retweets


### Quote Tweets

Here is an example of an extended Quote of a extended Tweet:
https://twitter.com/SnowBotDev/status/938444746686480384


# Next Steps <a id="next" class="tall">&nbsp;</a>



=================

# Previous drafts




For example, below is an excerpt from the root-level of a Retweet. In native data format, Retweets can be identified by the presence of data in the ‘retweeted_status’ field.

```
{
  "created_at": "Thu Sep 07 18:40:45 +0000 2017",
  "id_str": "905863423942156288",
  "text": "RT @NASAClimate: #Irma will be passing over waters warmer than 86 degrees F\u2014 hot enough to sustain a Category 5 storm.\u2026 ",
  "user": {
    "id_str": "944480690"
   },
  "retweeted_status": {
    "created_at": "Thu Sep 07 18:09:11 +0000 2017",
    "id_str": "905855482501279745",
    "text": "#Irma will be passing over waters warmer than 86 degrees F\u2014 hot enough to sustain a Category 5 storm.\u2026 https:\/\/t.co\/jIs8TAv5Dx",
    "user": {
      "id_str": "15461733",
    },
    "extended_tweet": {
      "full_text": "#Irma will be passing over waters warmer than 86 degrees F\u2014 hot enough to sustain a Category 5 storm.\nhttps:\/\/t.co\/jdtYxdQyEk https:\/\/t.co\/OtH7XEkVKB",
    },
    "quote_count": 0,
    "retweet_count": 134,
    "retweeted": false,
  },
  "is_quote_status": false,
  "retweet_count": 0,
  "retweeted": false
 }
```

And here is an original Tweet:
 
 
Integrating Retweets
Once your app is able to properly identify Retweets, it needs to know which fields to use to retrieve the various pieces of data it needs. This can be confusing due to the multi-layered structure of a Retweet object.
Each Retweet contains two layers:
1) an outer layer, which holds data related to the Retweet action itself, and the user who performed the Retweet, and
2) and inner layer which holds data about the original Tweet, including data about the user who posted it.
[AS->native update]
The outer layer exists at the root-level of the JSON Tweet object – the root level – while the inner layer is contained within the root-level “object” field. More clearly, the Retweet is a Tweet object, which contains another whole Tweet object within the “object” field.
In the example below, this is represented by some excerpted fields, with Tweet ID 299935329132105728 being the “Retweet”, and 299935121384034304 being the original (the one being shared).
 
 
[AS->native update]
These layers contain essentially the same attributes – a field holding the text of the Tweet, an object to represent the user, a field for the time the Tweet was posted, etc. In Retweets, some of these are obviously different – e.g. the user who posted the original Tweet (represented in object.actor) is different than the user who Retweeted them (represented in the root-level actor).
In other cases, the content is similar. The best example of this is the ‘body’ or ‘text’ of the Tweet since Retweets are just sharing the unmodified text of the original. However, these fields are not identical due to Twitter’s handling of Retweets.
Specifically, the object.body field contains the original unmodified text of the Tweet which is being shared. In contrast, the root-level body field contains a slightly version of this which has been slightly modified by Twitter. For this, Twitter takes the original text and appends ‘RT @username’ at the beginning, using the username of the user who posted the original Tweet.
Additionally, in cases where the additional characters added by Twitter cause the length of the Tweet to exceed 140 characters, Twitter then truncates the text of the Tweet to make it fit, adding ellipses at the end. For example, the original text in the Tweet being shared in the Tweet above was:
 
However, in the Retweet, Twitter modifies the root-level “body” to look like the following.
 

In terms of filtering, for quote Tweets, content from both the original quoted Tweet and the new “comment” Tweet can be matched. This filtering change is focused on operators that match on the Tweet body, including keywords, contains:, phrases, proximity, @mentions, #hashtags, $cashtags, url_contains:, url:, has:links, has:mentions, has:media, has:hashtags, and has:symbols.
Quote Tweet filtering and the fully hydrated Quoted Tweet payload is available for PowerTrack 2.0, Replay 2.0 and Historical PowerTrack 2.0.
Note: Please visit the Sample Payloads section of the site to view a Quote Tweet in both Original and Activity Streams format.
For additional information on quoted Tweets please see the Twitter description HERE as well.
 
Retweet Counts and Favorite Counts
Twitter provides a retweetCount field in Tweets and Retweets. In Tweets (not Retweets), this count is always (or almost always) zero because Twitter sends them through the firehose at or near the time of creation, when there hasn’t been enough time for someone to Retweet them yet.
For Retweets, this represents the current retweet count for the Tweet being shared, at the time of that given Retweet. Note that this is the Retweet count provided by Twitter. Gnip doesn’t have insight into how this number is calculated, and it may be the case that it includes things like Tweets from protected accounts in the calculation, which is not possible with the public firehose provided to Gnip. The public firehose used by Gnip is limited to Tweets and Retweets from public Twitter accounts.
Favorite Counts also appear as described above, with the same qualifiers as Retweets. However, note that there is no favorite ‘activity’ sent through the firehose by Twitter, so you will not receive an update every time a user favorites a Tweet. In fact, you will only be able to get an updated favorite count for a Tweet each time that it is Retweeted. An alternative would be to update numbers separately from Gnip by sending requests to Twitter’s public REST API directly.
Additional Considerations
 
Note, however, that the Retweet and Favorite counts for these will not be instructive – they only relate to this ‘new’ Tweet, and will almost always be zero.
For any other questions related to identifying and integrating ReTweets or Quote Tweets, contact the Gnip support team.
 



Quote Tweets

Quote Tweets will have the same behavior as standard Tweets. If either the quoted Tweet or the original Tweet meets the new condition, then they will have the same extended_tweet / long_object object and/or display_text_range in the payload. The extended_tweet / long_object will be at the root-level if the quoted status meets the new condition and/or within the “quoted_status” section of the payload if the Tweet being quoted meets the new condition.

For clarity, below are the four scenarios that you can expect with Quote Tweets:

Neither the Quote Tweet nor the original Tweet meet the new condition: no extended_tweet / long_object present in the payload.
The Quote Tweet meets the new condition but original Tweet does not: presence of root-level extended_tweet / long_object in the payload.
The Quote Tweet does not meet the new condition but original Tweet does: presence of extended_tweet / long_object in “quoted_status” section of the payload.
Both the Quote Tweet and original Tweet meet the new condition: presence of extended_tweet / long_object in both the root-level and “quoted_status” section of the payload.
Retweets

Retweets function slightly different than the above behavior described for a Quote Tweet. If the Retweet status meets the new condition, then the extended_tweet / long_object object and/or display_text_range will only be present in the retweeted_status object of the payload (e.g., object.long_object for AS format). To properly get data from a Retweet with the new condition, your app should use the inner-most “body” field and extended_tweet / long_object objects in the payload (e.g., object.long_object).
