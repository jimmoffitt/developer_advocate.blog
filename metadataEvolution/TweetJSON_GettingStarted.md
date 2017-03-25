[

+ core details are here: http://support.gnip.com/sources/twitter/data_format.html#TweetActivities
+ How can this Getting Started Guide build on top of that?

+ Focus on 'original' format, the format native to Twitter. (e.g. not Activity Streams as designed by Gnip)


]

### Twitter JSON Objects 101 <a id="twitterJsonIntro" class="tall">&nbsp;</a>

Tweets are made up of a Tweet message, a posted time, a set of User (also referenced to as Author or Actor) attributes, a collection of engagement metadata, and sometimes geographical metadata. Twitter APIs provide Tweet metadata encoded in JSON, a simple data-exchange text format based on key/value pairs. Tweet JSON is made up various objects, objects that can have many attributes. For example, the following JSON represents select attributes of a Tweet object. The Tweet object is made up of other objects such as ```user```, ```entities``` and ```place``` . 


#### Core Tweet attributes

``` 
{
 "id": 831569219296882688,
 "created_at": "Tue Feb 14 18:22:06 +0000 2017",
 "text": "",
 "user": {},
 "entities": {},
 "place": {}
}
```
+ Unique Tweet ID

+ Time Posted

+ Tweet body
 
+ User object
   + Handle
    + Display Name
    + Bio
    + Location
    + URL
    + Enrichments
    
+ Twitter entities
   + hashtags
   + mentions
   + symbols
   + urls
   + media

+ Geo 
   + Tweet Geo
   + Profile Geo

+ Other Enrichments
    
### JSON Examples

#### Example Tweet JSON

#### Example User JSON

#### Example Geo JSON


