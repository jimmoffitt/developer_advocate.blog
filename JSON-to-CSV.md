**JSON-to-CSV Conversion notes**

#"Can you deliver the data in CSV?"#

As a developer advocate at Gnip I often get asked about converting JSON data to CSV. The user-story behind this question comes primarily from one-time consumers of historical social media data. A common scenario is a researcher (likely from a non-computer field) who needs to import hundreds of thousands (if not millions) of tweets into some established data-store. Many of these data warehouses can readily import statically structured data such as CSV. 

CSV is probably the most prevalent format for transfering data from one system to another.

It is very common to store social activites in a single database table.  CSV map easily to tables.

Many users want to play with the data in Excel. 
Hydrology [tweeting-in-the-rain example], Social Science, Policital Science, Marketing, Sales


##Some Background##

Comma-Separated-Values (CSV) formating is fundamentally static, a two-dimensional grid of data and information.  All spreadsheet software readily imports CSV-formatted.  CSV data is also easy to import into database as tables. There is essentially a one-to-one relationship between CSV files and data tables.

JSON formatting is dynamic in nature because it readily supports hashes and arrays of variable length. In this sense JSON can natively represent an additional dimension.  There is essentially a one-to-many relationship between JSON files and (multiple) data tables.

+ CSV = 2D = single database table
+ JSON = 3D = multiple database tables




<insert tweet graphic>
```
With a little luck, my tour next week includes #Vail #Breckenridge #Copper. Too bad I can't get to #AftonAlps.
```
</insert tweet graphic>

With all tweets come a large set of metadata. Many of these are 'atomic' in nature, where there is only one instance of many attributes:

The basics:

```
{
  "id": "tag:search.twitter.com,2005:403224522679009280",
  "verb": "post",
  "postedTime": "2013-11-20T18:13:12.000Z"
}
```

Actor object:

```
"actor": {
    "preferredUsername": "jimmoffitt",
    "friendsCount": 92
    "followersCount": 86,
    "favoritesCount": 102
  }
 }
```

The above tweet metadata can readily be converted to comma-separated values:

```
id,verb,postedTime,preferredUsername,friendsCount,followersCount,favoritesCount
tag:search.twitter.com,2005:403224522679009280,post,2013-11-20T18:13:12.000Z,jimmoffitt,92,86,102
```


**Twitter Hashtags: an example of storing arrays**

Twitter hashtags are fundamental to how Twitter works.  Hashtags make it possible to focus in and filter for topics and resources of interest. Of all the metadata the comes with tweets, hashtags are commonly a primary focus.  Below is an example of a tweet using multiple hashtags:

```
{"twitter_entities": {
    "hashtags": [
      {
        "text": "AftonAlps"
       },
      {
        "text": "Breckenridge"
       },
      {
        "text": "Copper"
       },
      {
        "text": "Vail"
       }
     ]
  }
}
```


**Converting JSON arrays to a single CSV field**



**Deciding what JSON attributes to convert: JSON tweet templates**

One fundamental challenge of converting social media data, regardless of formats, is the sheer size of the datasets. A single activity may consist of over 150 attributes. Even a dataset with 1,000,000 social activities, a relatively small dataset, then results in 150,000,000 attributes names and values.  



**Example Use-case**

The process of converting tweets from JSON to CSV was much more complicated than anticipated. To help illustrate the path taken for this project, we'll tell a short data story. We'll survey a 24-hour period looking for Twitter signals around New Years Eve 2014 in three different parts of the world.  

Exploring News Year 



**********************************************
**Example Code**


Fundamental details




*Nominating JSON arrays for 'special' flattening*

config@arrays_to_collapse = 'hashtags,user_mentions,twitter_entities.urls,gnip.urls,matching_rules,topics'



*Supporting 'special' header mappings*

Default behavior is to use the dot-notation key by default, but some keys get silly-long:

    #twitter_entities.hashtags.0.text               --> hashtags
    #twitter_entities.urls.0.url                    --> twitter_urls
    #twitter_entities.urls.0.expanded_url           --> twitter_expanded_urls
    #twitter_entities.urls.0.display_url            --> twitter_display_urls
    #twitter_entities.user_mentions.0.screen_name   --> user_mention_screen_names
    #twitter_entities.user_mentions.0.name          --> user_mention_names
    #twitter_entities.user_mentions.0.id            --> user_mention_ids
    #gnip.matching_rules.0.value                    --> rule_values
    #gnip.matching_rules.0.tag                      --> tag_values
    
The above represent the current defaults that are automatically generated. These can be updated in the (YAML) config file.

These are specified at config@header_mappings.
