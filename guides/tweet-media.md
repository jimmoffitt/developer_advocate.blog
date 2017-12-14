+ [] Add in tips for using URL enhanced metadata.

# Matching on and parsing Tweets with photos and videos

+ [Introduction](#intro)
+ [Matching on Tweets with native media](#match-native)
+ [Parsing Tweets with native media](#parse-native)
+ [Matching on Tweets with linked media](#match-linked)
+ [Parsing Tweets with linked media](#parse-linked)
+ [Media metadata timeline](#history)  
+ [Next steps](#next)

## Introduction <a id="intro" class="tall">&nbsp;</a>
Millions of Tweets with photos, videos and animated GIFs are posted every day. We often hear from customers and developers with questions on how to find and process Tweets with media of interest. The purpose of this guide is to help you do just that.

When working with premium and enterprise APIs that return Tweets, a first step is developing rules, or filters, for finding Tweets of interest. These filters are built using a simple querying language, based on boolean syntax and a set of [premium operators](https://developer.twitter.com/en/docs/tweets/search/guides/premium-operators). When needing to match on media, there are two small sets of operators to work with. Picking the appropriate set of operators depends on how the media is shared.

The first set is used to match Tweets that include photos, videos, and animated GIFs “attached” to the Tweet when composing it with the Twitter user-interface. These media are referred to as *native* media. Here's an example of a Tweet with native media:
https://twitter.com/SnowBotDev/status/936075836607705089

The second set is used to match media that is included as a link to some resource hosted off the Twitter platform. Here is an example of a Tweet including a link to a photo hosted elsewhere (in this case github.com):  
https://twitter.com/SnowBotDev/status/938444746686480384

We will start off with Tweets with *native media*. As the Twitter platform has evolved, native media has become the most common source of Tweets with media. We will introduce the Premium operators provided for matching on that content, walk through some example filters, and point you to the Tweet JSON attributes that contain native media metadata.  

Then we'll turn our attention to Tweets that contain *linked media*. Sharing photos and videos hosted on other networks and platforms is still a common use of Twitter. Also, if you are working with historical Tweets, being familiar with linked media is critical since native Twitter videos were launched in 2016 (and native photos in 2011). We will discuss the Premium operators available for surfacing these Tweets of interest, discuss a few example filters, and introduce the Tweet JSON attributes available for shared links.

Finally, if you are collecting Tweets with media from before June 2016 using one of our historical APIs, be sure to review the "Media metadata timeline" section below. It provides a summary of important "born-on-dates" for Tweet media metadata, and provides links to key documentation for building effective, timeline-aware filters. 

## Matching on Tweets with *native media* <a id="match-native" class="tall">&nbsp;</a>

{The vast majority of shared media is attached to the Tweet when it is getting composed. With native Twitter media it is easy to separate photos and videos into different types. } 

The following premium operators are available for matching on Tweets with native media:

+ ```has:images```: Matches all Tweets that contain native photos (up to four).
+ ```has:videos```: Matches all Tweets that contain native videos and animated GIFs. *Note that these do not match on videos from Vine or Periscope*.
+ ```has:media```: Matches all Tweets that contain any native media (photos, video, or animated GIF). Note that the rule clause ```has:media``` is equivalent to ```has:images OR has:videos```.

### Example native media filters

To illustrate how these native media operators can be used, here are some example Tweet matching 'use cases' and corresponding filters:

+ *"I want to collect all Tweets that my brand has posted with native photos. Using the [enterprise engagement API], we want to measure which Tweets received the most engagement."* In order to collect all of these Tweets for analysis, the following filter could be applied with either historical API:

```from:MyBrandAccount has:photos```

+ *"I am interested in any Tweet with native video or photos that mentions my brand or product."*

```(@MyProduct OR #MyProduct OR MyProduct OR "my product nickname") has:media ``` 

+ *"I am interested in Tweets about winter weather and include native videos."*

```(snow OR snowing OR blizzard OR (winter (watch or weather))) has:videos```


## Parsing Tweet JSON with native media <a id="parse-native" class="tall">&nbsp;</a>

For Tweets with native media, media metadata is provided in the ```extended entities``` JSON object. The ```extended entities``` object is the go-to resource for *all* native media. See our [```extended entities``` documentation](https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/extended-entities-object) for example JSON payloads for Tweets with photos, videos and animated GIFs. 

As described, the ```extended entities``` object provides a ```media[]``` array. In the case of photos, this array can have up to four items, while with native videos and animated GIFs there can only be a single item. Note that when composing a Tweet, only one type of media can be attached (photo or video or GIF). In turn each media item has a top-level ```type``` attribute that indicates what kind of media is attached. The ```type``` attributes will be set to either ```photo```, ```video```, or ```animated_gif```. A quick reminder that the ```has:video``` operator will match on both videos and animated GIFs. If you have a use case that focuses only on videos, you need to inspect this ```type``` attribute to segregate the two types of media.

You may notice a ```media``` entry in the ```entities``` object, but that object should be *avoided and ignored when parsing native media metadata*. A ```media``` section was added to the standard ```entities``` object in August 2011 when Twitter enabled the attachment of a single photo. When Twitter began supporting up to four photos in March 2014, and when native GIFs and videos were introduced in 2016, the ```entities.media``` object was *not* updated, and instead these new metadata were provided in the ```extended_entities``` JSON object. The old ```entities.media``` will always indicate the native media as a single 'photo', even though the native media may consist of multiple photos or a video or an animated GIF.    

## Matching on Tweets with *linked media* <a id="match-linked" class="tall">&nbsp;</a>

Matching on linked photos and videos is a bit more challenging than matching on native media. With linked media, all we have is metadata about the linked URL, and its home page HTML title and description. With links, there is no *media* metadata (display sizes, aspect ratios, etc.). One challenge with linked media is that it can be difficult to segregate media types at the filtering level. While some hosting sites focus primarly on videos *or* photos, more and more sites support both. Accordingly, if you have a use case with a focus on videos, you should anticipate needing to further filter out the many photos that are likely to match your rule.

The good news is there are strategies for building effective filters.  

The following premium operators are available for matching on Tweets with links: 

+ ```url:``` - Most common operator for matching on Tweets with linked media. This operator matches on specified URL tokens and phrases. Supported in all premium and enterprise historical and real-time APIs. 

+ ```url_contains:``` - Matches on URL *substrings* and available in non-search APIs: only available with *batched* historical and real-time APIs.   

+ ```has:links``` - Not recommended for matching on Tweets with linked photos and videos, as it is too general. Instead the ```url:``` operator can be used to match tokens from media hosting services of interest.  

The ```url:``` operator is the most useful way to match on linked media. When using this operator, any operand that contains punctuation should be double-quoted. In this context the most common usage is to curate a list of URL tokens and phrases that reference media hosting platforms of interest. Common tokens include ```flickr```, ```youtube```, ```photobucket```, ```photos.google```, and ```instagram```. 

As indicated above, the ```has:links``` operator is not generally recommended for matching on Tweets with photos and videos. The ```has:links``` operator return all Tweets that has a link in the Tweet body, regardless of what it is linking to. This includes links to non-media resources such as news articles, product pages, and other web link. This includes any media uploaded to Twitter, because a pic.twitter.com URL is generated when a Twitter user uploads a photo, but it is certainly not limited to photos. Used by itself, the has:links operator returns a very large volume of Tweets. If you want to target Tweets with photos and videos, using this too general operator will generate a lot of noise. For that reason, the has:links should only be used in combination with keywords or other operators that more specifically target the content you want.

### Example linked media filters

+ *"I am interested in any Tweet with linked video that mentions my brand or product."*  

```(@MyProduct OR #MyProduct OR MyProduct OR "my product nickname") (url:youtube OR url:vimeo)```

+ *"I am interested in Tweets about winter weather and include linked photos."*

```(snow OR snowing OR blizzard OR (winter (watch or weather))) (url:flickr OR url:"photos.google" OR url:instagram OR url:photobucket)```

## Parsing Tweet JSON with linked media <a id="parse-linked" class="tall">&nbsp;</a>

For Tweets with linked media, you will need to parse the ```urls``` section of the [entities object](https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/entities-object). The ```entities.urls``` array is the go-to resource for *all* Tweet links, including links to videos and photos. The ```entities.urls``` attribute provides an array URL metadata, one for each link included in the Tweet. This URL metadata includes the display and expanded URLs, and if [expanded and enhanced URLs](https://developer.twitter.com/en/docs/tweets/enrichments/overview/expanded-and-enhanced-urls) is enabled for your account, a fully unwound URL along with the linked page's HTML title and description.     

Note that the ```extended entities``` section will not provide any additional media metadata for these links. 

## Media metadata timeline <a id="history" class="tall">&nbsp;</a>

If you are doing historical research with Tweets, there are several key dates and details to understand before making requests with our historical Tweet APIs. As detailed [HERE](https://developer.twitter.com/en/docs/tweets/data-dictionary/guides/tweet-timeline), the Twitter platform has evolved continually since 2006.

In the early days of Twitter the only way to share media was to include a link to content hosted on other sites. Starting in August 2011, Twitter users could start 'attaching' photos to Tweets with the user interface. In March 2014, up to four photos could be included in a Tweet. In June 2016 videos and animated GIFs became supported. Again, when media is 'attached' to a Tweet using the Twitter user-interface, it is said to be *"native"* media, hosted on the Twitter platform.  

 When it comes to matching Tweets with media, the following dates are fundamental to designing effective filters:

### Native media timeline
+ August 2011 - Native photos introduced.
+ March 2014 - Support for up to four photos, and the introduction of the extended entities JSON metadata.
+ February 2016 - GIFs natively hosted in Tweet compose.
+ June 2016 - native video begins

### Linked media timeline
+ Since the start of Twitter URLs have been included in Tweets. 
+ In March 2012, the [expanded URLs] data enrichment was introduced. This enrichment provided fully unwound URLs with the ```url:``` operator matching on those unwound URLs. For Tweets posted before March 2012, the ```url:``` will only match against URLs as they were entered by the user and not the fully resolved URL (i.e. if a bit.ly URL is entered in the Tweet it can only match against the bit.ly and not the URL that has been shortened by bit.ly)

Note that there are many other product-specific related details to consider when designing filters to match historical Tweets. If you are using full-archive search, see [this guide](https://developer.twitter.com/en/docs/tweets/search/guides/fas-timeline) for a time-line of when new Tweet JSON attributes, and corresponding filters operators, were added. If you are using our batch historical API, see [this guide](https://developer.twitter.com/en/docs/tweets/batch-historical/guides/hpt-timeline). Also, see [this tutorial](https://developer.twitter.com/en/docs/tutorials/choosing-historical-api) for help with deciding which historical API is right for you.  

## Next steps <a id="next" class="tall">&nbsp;</a>

+ Learn more about Tweet JSON. See our data dictionaries for the [```entities```] and [```extended entities```] JSON objects.
+ Learn more about [premium operators].
+ Learn more about identifying and matching on Retweeted and Quoted Tweets.     
+ Learn more about Twitter's evolution and how that affects historical research with Tweet data.

 


