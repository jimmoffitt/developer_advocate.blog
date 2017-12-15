
[Getting major overhaul, to have narrative/examples change from AS to original format. Correcting/updating Quote Tweet content. Adding in relevant b140 and #280 details]

Example Tweets:
https://twitter.com/FloodSocial/status/928950635860545537

Simple quote Tweet
https://twitter.com/SnowBotDev/status/925390480744923136


Extended quote Tweet x 2
https://twitter.com/FloodSocial/status/936771032077107200

Extreme example: https://twitter.com/SnowBotDev/status/941414422920097797


To-dos/Tests
{Need to discuss retweet and quote counts and describe which ones include both...}



+ [Introduction](#intro)
+ [How to match on Retweets and Quote Tweets](#match)
+ [Parsing Retweets and Quote Tweets](#parse)
  + [Parsing extended Tweets](#parse-extended)
  + [Parsing Retweets](#parse-retweet)
  + [Parsing Quote Tweets](#parse-quote)
+ [Next Steps](#next)

# Introduction <a id="intro" class="tall">&nbsp;</a>

Twitter API customers often want to know the specifics around identifying and integrating Retweets and Quote Tweets into their products, but can run into a few common roadblocks. If you’re looking for the best way to incorporate Retweets and Quote Tweets into your product or analysis, this guide will provide everything you need to know about identifying them, and best practices for extracting the information you need from them. 

Retweets are an important part of Twitter’s platform – they permit content to be shared rapidly and with attribution, and are the most easily measured form of content engagement on the platform. Quote Tweets, which are like Retweets except they included added content,  were introduced in 2015, and have become a widely used method for sharing, and commenting on, Tweets. Many social analytics tools use the number of Retweets and Quote Tweets a particular Tweet receives in calculating its impact or reach (i.e. its importance). However, to do so, your app must be able to accurately identify these.

While *matching* on Retweets and Quote Tweets is straight-forward, *parsing* the corresponding JSON objects has some challenges. First, Every Retweet and Quote Tweet JSON payload contains two Tweet objects, complete with two user objects. Keeping these straight is critical. Second, the introduction of #280 characters, and 'extended' Tweets before that, created more complexity. When these #280 payload changes were made, they were made in a backwards-compatible fashion, preserving all the JSON fields long used to represent Tweets with 140 or fewer characters, while adding new attributes that completely encapsulate #280 Tweets. For example, the Tweet JSON root-level ```text``` attribute remains and still only handles 140 characters. To support #280, a root-level ```extended_tweet``` object was added. This new object appears when the Tweet it describes has more than 140 characters, and the full message is provided in the ```extended_tweet.full_text``` field.   

What this means is that if you have a parser that has not been updated for #280 Tweets, it will still correctly handle Tweets with 140 or less characters, but will absolutely fail to capture complete metadata for Tweets with more than 140 characters. First, the text of the Tweet message will be truncated and will not contain the entire message. 

Second, the introduction of #280 also means that there are up to four ```entities``` objects in a Quote Tweet, and up to three in a Retweet. The  ```entities``` object contains fundamental Tweet details, such as hashtags, cashtags, mentions, and links. With #280 Tweets, the root-level ```entities``` object will most likely be incomplete. For example, if a Tweet contains hashtags or links beyond the 140-character mark (and most probably do), the ```entities``` metadata will not include them. Instead, you'll need to access the ```extended_tweet.entities``` object to guarantee complete data. Knowing the correct ```entities``` object to parse is critical for completely surfacing hashtags, links, and other fundamental Tweet attributes of interest. 

Let's start off with some fundamental descriptions. Then we'll discuss how to match Retweets and Quote Tweets of interest. Then we'll dive into the many details involved when designing a Retweet and Quote Tweet parser.

### What is a Retweet?

A Retweet is an action taken by a Twitter user to share another user’s Tweet without alteration, using Twitter’s explicit Retweet functionality. When you Retweet, there are no options to add any comments, hashtags, links, or any other details. 

https://twitter.com/SnowBotDev/status/928707908354891781 
Redirects to: https://twitter.com/Arapahoe_Basin/status/928290029436248064

A Retweet retains information about the user who posted the original Tweet, as well as the user who Retweeted them. 

### What is a Quoted Tweet?

Quote Tweets are another way of sharing Tweets that includes *adding* your own new content as a comment. Quote Tweets can be selected after using Twitter’s Retweet option. 

https://twitter.com/SnowBotDev/status/925390480744923136

In some ways Quote Tweets can be thought of as a special kind of Retweet. They retain information about the user who posted the Tweet being quoted, as well as the user who Quoted them and added new content. 


# How to match on Retweets and Quote Tweets <a id="match" class="tall">&nbsp;</a>

If you are using a Twitter premium or enterprise search API, there are two operators for matching Retweets:

+ ```is:retweet```: {define}
+ ```retweets_of:```: {define}

If you are using our enterprise real-time or batched historical APIs, there is this additional Retweet-targetting operator:

+ ```retweets_of_status_id:```: {define}

The ability to match on whether the Tweet is a Quote Tweet depends on what API you are using. Again, if you using the enterprise real-time or batched-historical APIs, then this operator is available:

+ ```is:quote```: {define)

If you are using the search APIs, this operator is not available. So, with search you can not build a rule that matches only on Quote Tweets. 

There are other fundamental Quote Tweet matching differences between the search APIs and the real-time/batched-historical APIs. With the enterprise real-time and batched-historical APIs, the both additive Quote and Quoted contents are tested to match your rules. With the search APIs only the additive Quote contents are tested to match your rules. Take the following Quote Tweet as an example:

https://twitter.com/SnowBotDev/status/941448912954462208

With the real-time and batched-historical APIs, this Quote Tweet would match the rule ```from:SnowBotDev #COwx```. That same rule would not match with the search APIs since the Quoted Tweet (with the hashtag #COwx) is not referenced with applying the rule. 

Regardless if you are explicitedly matching on Retweets or Quote Tweets or not, you will certainly have Retweets and Quote Tweets to parse, analyze, or display. In the next section, we'll discuss how to correctly parse these Tweets to ensure your are extracting complete metadata.

# Parsing Retweets and Quote Tweets <a id="parse" class="tall">&nbsp;</a>

{This discussion is based on the *native* Tweet JSON format.}

As stated above, there are challenges when parsing Retweets and Quote Tweets. The complications mainly stem from the introduction of #280 Tweets and the ```extended_tweet``` objects. A first step in correctly handling Retweets and Quote Tweets is ensuring your parser is correctly handling extended Tweets. With extended Tweets there are legacy fields that will contain truncated, thus incomplete, information. These truncated fields include fundamental Tweet attributes such as the message itself and hashtags, mentions, and links included in that message.  

## Parsing extended Tweets <a id="parse-extended" class="tall">&nbsp;</a>   

Building a parser to handle extended Tweets is straightforward. Every Tweet object has a root-level ```truncated``` boolean field. When this field is set to 'false', the Tweet message has 140 or less characters and only the legacy fields are provided since they completely describe the Tweets. However, when ```truncated``` is set to 'true', there will be a root-level ```extended_tweet``` object, and the root-level ```text``` field and ```entities``` object should be ignored. Instead, the ```extended_tweet.full_text``` value will provide the full Tweet message, and the ```extended_tweet.entities``` will provide complete arrays of hashtags, mention, links, and other ```entities``` contents.      

## Parsing Retweets <a id="parse-retweet" class="tall">&nbsp;</a>   

When the Tweet JSON you are parsing has a root-level ```retweeted_status```, you are working with a Retweet. This ```retweeted_status``` object is a complete Tweet object that represents the original Tweet being Retweeted. The ```retweeted_status``` object in turn will reflect whether the original Tweet was an extended Tweet or not. 

Since Retweets encapsulate two Tweet objects, there are three possible scenarios that can affect successful parsing of Retweet contents:

+ Retweet of a non-extended Tweet that does not result in a truncated RT message. 


```json
{
  "id_str": "928707908354891781",
  "text": "RT @Arapahoe_Basin: The morning views never disappoint. https:\/\/t.co\/BWfhLHW8qV",
  "truncated": false,
  "user": {
    "id_str": "906948460078698496"
  },
  "retweeted_status": {
    "id_str": "928290029436248064",
    "text": "The morning views never disappoint. https:\/\/t.co\/BWfhLHW8qV",
    "user": {
      "id_str": "15537164"
    },
    "entities": {}
  },
  "entities": {}
}

```

+ Retweet of a non-extended Tweet that does result in a truncated RT message. Retweets have the "RT @screen_name " pattern prepended to the original Tweet message. That extra text added to the original message often results in a message longer than 140 characters, causing the root-level ```text``` field to truncate the original message. 

```
{
  "id_str": "941455716254064640",
  "text": "RT @FloodSocial: Nothing 2 see here. Just a test Tweet that is exactly 140 characters. This will be Retweeted in the #280 world to see what\u2026",
  "truncated": false,
  "user": {
    "id_str": "906948460078698496"
  },
  "retweeted_status": {
    "id_str": "941455227248455680",
    "text": "Nothing 2 see here. Just a test Tweet that is exactly 140 characters. This will be Retweeted in the #280 world to see what happens #entities",
    "truncated": false,
    "user": {
      "id": 944480690
    },
    "entities": {
      "hashtags": [
        {
          "text": "entities"
        }
      ]
    }
  },
  "entities": {
    "hashtags": [
      
    ],
    "user_mentions": [
      {
        "id_str": "944480690"
      }
    ]
  }
}

```


+ Retweet of a extended Tweet


```


```


Since new content can not be added to a Retweet, the root-level ```entities``` object should always be ignored. Instead Twitter entities should be parsed from the ```retweeted_status``` object, and which ```entities```depends on whether it is an extended Tweet that was Retweeted. If ```retweeted_status.truncated``` is 'true' the ```retweeted_status.extended_tweet.entities``` should be parsed, and the root-level ```retweeted_status.entities``` object ignored.  If ```retweeted_status.truncated``` is 'false', the ```retweeted_status.entities``` object should be parsed (and is the only one provided).
    
## Parsing Quote Tweets <a id="parse-quote" class="tall">&nbsp;</a>    
    
Since Quote Tweets encapsulate two Tweet objects, there are four possible combinations of non-extended and extended Tweets object in a Quote Tweet JSON payload:
    
+ Non-extended Quote of non-extended Tweet
+ Extended Quote of non-extended Tweet
+ Non-extended Quote of extended Tweet
+ Extended Quote of extended Tweet









```json

{
	"created_at": "Sun Sep 24 23:58:20 +0000 2017",
	"id_str": "912103941030141952",
	"text": "news that warms my heart https://t.co/HhnVdflF7k",
	"user": {
		"id_str": "906948460078698496"
	},
	"quoted_status_id_str": "911742658460057600",
	"quoted_status": {
		"created_at": "Sun Sep 24 00:02:43 +0000 2017",
		"id_str": "911742658460057600",
		"text": "#Snow!!! Here's the scene from Peak 7! #Breck #CoWx https://t.co/v0eNkbCYor",
		"user": {
			"id_str": "19746495"
		},
		"is_quote_status": false,
		"quote_count": 0,
		"retweet_count": 105,
		"retweeted": false
	},
	"is_quote_status": true,
	"quote_count": 0,
	"retweet_count": 0,
	"retweeted": false
}

```


Two forms of Tweet JSON are available. This content will focus on the Twitter "native" (or "original") format. See HERE for the first version of this content, written for the Activity Streams format.

## Many possible combinations

### Retweets

Retweet of a non-extended Tweet

```
{
"retweeted_status":"text"
"retweeted_status":"entities"
}
```

Retweet of an extended Tweet

```
{
"retweeted_status":"extended_tweet":"full_text"
"retweeted_status":"extended_tweet":"full_text"
}
```


### Quote Tweets

Extended Quote of an extended Tweet:
https://twitter.com/SnowBotDev/status/938444746686480384



Quote - Quote 

```
{
"text"
"entities"
"quoted_status":"text"
"quoted_status":"entities"
}
```



*Extended* Quote - Quote 

```
{
"extended_tweet":"full_text"
"extended_tweet":"entities"
"quoted_status":"text"
"quoted_status":"entities"
}
```

Quote - *extended* Quote 

```
{
"text"
"entities"
"quoted_status":"extended_tweet":"full_text"
"quoted_status":"extended_tweet":"entities"
}
```


*Extended* Quote - *extended* Quote

```
{
"extended_tweet":"full_text"
"extended_tweet":"entities"
"quoted_status":"extended_tweet":"full_text"
"quoted_status":"extended_tweet":"entities"
}
```






### Retweets


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






# Next Steps <a id="next" class="tall">&nbsp;</a>



=================

# Previous drafts


 
 
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
