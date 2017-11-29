
* [] Add: has:images, has:videos
* [] Discussion of url needs: differences between url: and url_contains: (product, token matching)
* [] Intro to enhanced URL unwinding/filtering, or reference to new page (next steps)?

Does has:links match on a photo attachment? yes
Any differences between Search/PT?
Mention of extended_entities as place to go for any media parsing. 

--------------------------
# Matching on Tweets with photos and videos

## Introduction

More and more frequently, Tweets include including photos, videos and animated GIFs. On Twitter there are two ways to share these types of media. You can 'attach' media with the Twitter user-interface, or you can include a link to a media hosting platform such as YouTube, Instagram, Flickr, or Vimeo.

In the early days of Twitter the only way was to include a URL link to content hosted on other sites. Starting in 

Today, the most common method is to 'attach' media content while composing a Tweet. Photos in August 2011 (up to four photos in March 2014), and videos in June 2016.


(To learn more about the evolution of sharing media on Twitter, see [HERE](https://developer.twitter.com/en/docs/tweets/data-dictionary/guides/tweet-timeline).) 

When media is attached to a Tweet using the Twitter user-interface, is said to be "native" media, hosted on the Twitter platform.  

Twitter’s Premium filtering language gives you the ability to filter the Twitter Firehose for data that is relevant to you, your research or your brand. PowerTrack provides Operators that enable you to match on the Tweet attributes of interest. PowerTrack provides a firehose filtering syntax that enables users who want Tweets with photos, videos that contain the keyword 'snow' or 'rain' or 'flood' to write PowerTrack filters such as:
 (snow OR rain OR flood) has:videos
 
## Premium Operators for matching on media

The following for premium operators are available when wanting to match on Tweets that include media, whether attached natively, or linking to an external media resource:


+ has:media
+ has:images
+ has:videos


+ has:links



 
The has:media and has:links operators are two options in PowerTrack that can be useful in matching Tweets that contain links to media. However, there are some significant differences in how they function, and what they’ll return. The has:media has a far narrower scope than has:links.
 
## Native media

The other class focuses on native media (made up of photos and videos) and includes has:media, has:videos, and has:images.  Note that the has:videos Operators matches on GIFs, and the metadata included with a Tweet indicates whether it was a video or GIF. Also, the rule clause of has:media is the same as (has:images OR has:videos).

has:media
[UPDATE: Specifically, has:media only looks for Tweets with content in the twitter_entities.media field, which only ever includes pic.twitter.com links for images uploaded directly through Twitter, as of the time of writing. This could change in the future, if Twitter begins including more types of content in the “media” entity, but since photos are the only media that Twitter allows to be directly uploaded by the user today, there are no references to other types or sources of media.]
has:media
has:images (returns all tweets that contain native images (e.g. pic.twitter.com))
has:videos (returns all tweets that contain native videos (does not include vine, periscope))
 
## Media hosted elsewhere
One class focuses on URLs in Tweets, and includes has:links, url: and url_contains: Operators.
has:links
The has:links operator, on the other hand, will return any activity that has a link in the Tweet body, regardless of what it is linking to. This includes any media uploaded to Twitter, because a pic.twitter.com URL is generated when a Twitter user uploads a photo, but it is certainly not limited to photos. Used by itself, has:links simply returns any activity that includes a URL, which can be a large volume of poorly-targeted data if you only care about Tweets with images or videos. For that reason, the has:links should only be used in combination with keywords or other operators that more specifically target the content you want.
But what if you and your brand is interested in knowing every time a customer Tweets a photo about your company or product, regardless of whether it was uploaded directly to Twitter or another popular social platform? For example, what if a Twitter user uploaded a photo to Flickr, and then shared the link on Twitter? A rule simply using the has:media operator would miss this Tweet, and the has:links operator would deliver it, but would also flood you with large volumes of irrelevant content. This is where the url_contains: operator is helpful.
url_contains
The url_contains: operator is the most useful way to filter for media that is not covered by has:media. The url_contains: operator matches on URL substrings. It can be enclosed in quotes to allow for the top level domain to be included in the query. For example, you could filter on:
 
url:"flickr.com"
This particular search would return activities where there is a link from flickr.com. On the other hand, if you’re merely interested in any time your product or company appears in a URL in a Tweet, you could do this:
url:PiedPiper
This take on the url: operator would return any activity where “PiedPiper” token appears anywhere in the URL - whether it is from PiedPiper.com or even someting like this:
http://www.networkworld.com/community/blog/valley-startup-spotlight-piedpiper-makes-social-media-fire-hose-seem-small

Going back to the scenario presented above, if you wanted to track Tweets where a photo was posted to Twitter in a Tweet that mentioned your company or product, you could use the following syntax.
(PiedPiper OR url:PiedPiper) (url_contains:"flickr.com" OR has:media)

You could then add additional ```url:``` terms to the second group for other image hosting services you wanted to capture. This also applies to video-hosting services – you would simply need to identify the structure used by links from that service and incorporate it into an additional url_contains term.
 
## Next steps

+ Learn more about PowerTrack Operators
+ Learn more about identifying and matching on Retweeted and Quoted Tweets.     
+ Learn more about Twitter's evolution and how that affects historical research.
