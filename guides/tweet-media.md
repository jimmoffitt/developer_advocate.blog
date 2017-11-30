
* [X] Add: has:images, has:videos
* [] Discussion of url needs: differences between url: and url_contains: (product, token matching)
* [] Intro to enhanced URL unwinding/filtering, or reference to new page (next steps)?
+ [X] Does has:links match on a photo attachment? yes
+ [X] Any differences between Search/PT?
+ [] Mention of extended_entities as place to go for any native media parsing. 

--------------------------
# Matching on Tweets with photos and videos

## Introduction

More and more frequently, Tweets include photos, videos and animated GIFs. On Twitter there are two ways to share these types of media. You can 'attach' media with the Twitter user-interface, or you can include a link to a media hosting platform such as YouTube, Instagram, Flickr, or Vimeo.

In the early days of Twitter the only way to share media was to include a URL link to content hosted on other sites. Starting in August 2011, Twitter users could start 'attaching' photos to Tweets with the user interface. In March 2014, up to four photos could be included in a Tweet. In June 2016 videos and animated GIFs became supported. (To learn more about the evolution of sharing media on Twitter, see [HERE](https://developer.twitter.com/en/docs/tweets/data-dictionary/guides/tweet-timeline).) When media is attached to a Tweet using the Twitter user-interface, is said to be "native" media, hosted on the Twitter platform.  

Twitter’s premium and enterprise search APIs provide operators for matching on Tweets with media. First, we will discuss three operators (```has:media```, ```has:photos```, and ```has:videos```) that match on native media. Then we'll discuss the ```url:``` operator and strategies for matching on Tweets with links to media hosted somewhere other than the Twitter platform. We'll wrap up our discussion with some examples rules using these operators.

## Native media
 
### Matching Tweets with native media

The following premium and enterprise operators are available when wanting to match on Tweets with native media:

+ ```has:media```: Matches all Tweets that contain any native media (photos, video, or animated GIF).
+ ```has:images```: Matches all Tweets that contain native photos (up to four).
+ ```has:videos```: Matches all Tweets that contain native videos (does not include videos from Vine or Periscope).

Note that the has:videos Operators also matches on GIFs, and the metadata included with a Tweet indicates whether it was a video or GIF. Also, the rule clause of has:media is the same as (has:images OR has:videos).

### Parsing Tweet JSON for native media metadata
{Always parse the extended_entities object.}
https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/extended-entities-object


## Media hosted elsewhere

### Matching Tweets with linked media

+ ```url:```

The url: operator is the most useful way to filter for media that is hosted elsewhere. The url: operator matches on URL *tokens*. It can be enclosed in quotes to allow for the top level domain to be included in the query. For example, you could filter on:

Note about the ```url_contains:``` operator:

Enterprise real-time and batched historical APIs support the ```url_contains:``` operator, which can match on URL *substrings*.

Note about ```has:links``` operator:

The has:links operator, on the other hand, will return any Tweet that has a link in the Tweet body, regardless of what it is linking to.
This includes any media uploaded to Twitter, because a pic.twitter.com URL is generated when a Twitter user uploads a photo, but it is certainly not limited to photos. Used by itself, the has:links operator returns a very large volume of Tweets. If you want to target Tweets with photos and videos, using this too general operator will generate a lot of noise. For that reason, the has:links should only be used in combination with keywords or other operators that more specifically target the content you want.




### Parsing Tweet JSON for linked media metadata

{Always parse the entities object.}

https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/entities-object



 

 
## Examples 

 
If you and your brand are interested in knowing every time a customer Tweets a photo about your company or product, regardless of whether it was uploaded directly to Twitter or another popular social platform? 
 
 url:"flickr.com"
This particular search would return activities where there is a link from flickr.com. On the other hand, if you’re merely interested in any time your product or company appears in a URL in a Tweet, you could do this:
url:PiedPiper
This take on the url: operator would return any activity where “PiedPiper” token appears anywhere in the URL - whether it is from PiedPiper.com or even someting like this:
http://www.networkworld.com/community/blog/valley-startup-spotlight-piedpiper-makes-social-media-fire-hose-seem-small

Going back to the scenario presented above, if you wanted to track Tweets where a photo was posted to Twitter in a Tweet that mentioned your company or product, you could use the following syntax.
(PiedPiper OR url:PiedPiper) (url:"flickr.com" OR has:media)

You could then add additional ```url:``` terms to the second group for other image hosting services you wanted to capture. This also applies to video-hosting services – you would simply need to identify the structure used by links from that service and incorporate it into an additional url_contains term.
 
 
## Next steps

+ Learn more about Tweet JSON. See our data dictionaries for the [```entities```] and [```extended entities```] JSON objects.
+ Learn more about [premium operators].
+ Learn more about identifying and matching on Retweeted and Quoted Tweets.     
+ Learn more about Twitter's evolution and how that affects historical research with Tweet data.
