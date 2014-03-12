#"Can you deliver the data in CSV?"#

At Gnip we often get asked about converting [JSON] (http://json.org) data to [CSV] (http://en.wikipedia.org/wiki/Comma-separated_values). The user-story behind this question comes primarily from one-time consumers of historical social media data. A common scenario is a researcher (likely from a non-computer field) who needs to import hundreds of thousands (if not millions) of tweets into some established data-store. Many of these data warehouses can readily import statically structured data such as CSV. The most common examples are database tables and even spreadsheets. 

Since CSV is probably the most prevalent format for transferring data from one system to another it is not too surprising how often this question comes up. It turns out this short question has a long answer. The main wrinkles are that JSON can readily store variable-length arrays of data and stores data at multiple 'levels' using potentially duplicate names. On the other hand, CSV data needs to have the same number of data fields per record. With CSV files there is an optional header that contains names corresponding to the individual fields in the file.     

We will begin our discussion by comparing and contrasting CSV and JSON, which will highlight the challenges of converting JSON data to CSV. Then we will dive into some Twitter data examples to help illustrate the conversion process. Finally, we will present some code used to tackle this problem.


##Some Background##

Comma-Separated-Values (CSV) formating is fundamentally static, a two-dimensional grid of data and information.  All spreadsheet software readily imports CSV-formatted.  CSV data is also easy to import into database as tables. There is essentially a one-to-one relationship between CSV files and data tables.

JSON formatting is dynamic in nature because it readily supports hashes and arrays of variable length. In this sense JSON can natively represent an additional dimension.  There is essentially a one-to-many relationship between JSON files and (multiple) data tables.

JSON is built with key names, often multiple levels deep, while CSV field names are determined by column position.


```
+ CSV = 2D = single database table
+ JSON = 3D = multiple database tables

+ With CSV, all data is represented by a string, while with JSON there are distinct numeric and string types.
+ With CSV the comma holds all the power.
+ With JSON, double quotes, braces, and commas equally share power.
```

To further the discussion, let's consider the following tweet:

<insert tweet graphic>
```
With a little luck, my tour next week includes #Vail #Breckenridge #Copper. Too bad I can't get to #AftonAlps.
```
</insert tweet graphic>

With all tweets come a large set of metadata. Many of these are 'atomic' in nature, where there is only one instance of many attributes. Here are some examples:

Tweet-specific attributes:

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


Note that with CSV there are a variety of ways to specify the header labels. In the above example, CSV header columns were based on the 'bottom-level' key name. 


Ah, but there is a wrinkle here. In JSON there can be duplicate key names. For example, there is an "id" attribute for both the tweet itself, but also for the 'actor' that posted it:

```
{"id": "tag:search.twitter.com,2005:403224522679009280",
 "actor": {
   "id": "id:twitter.com:1855784545"
  }
}
```

Given that, you can't just use the key name paired with the data value or you will end up with CSV headers that duplicate names:

```
id, id
tag:search.twitter.com,2005:403224522679009280,id:twitter.com:1855784545
```


| id        | id  |
| ------------- |:-------------:| -----:|
| tag:search.twitter.com,2005:403224522679009280     | id:twitter.com:1855784545 |

That is a recipe for disaster. Another option would have been to use dot notation, as in 'actor.perferredUsername'. Furthermore, you may want to remove redundant data from fundamental fields like 'id'. 

```
id, actor.id
403224522679009280,1855784545
```

However, sometimes using dot notation with tweet JSON payloads get a bit too verbose.  For example, next we'll discuss the JSON markup up for an array of metadata, such as Twitter hashtags.

##Twitter Hashtags: an example of storing arrays##

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


```
   twitter_entities.hashtags.0.text = AftonAlps
   twitter_entities.hashtags.1.text = Breckenridge
   twitter_entities.hashtags.2.text = Copper
   twitter_entities.hashtags.3.text = Vail
```

``` 
   twitter_entities.hashtags.0.text               --> hashtags
```

**Converting JSON arrays to a single CSV field**


[discuss in the context of a database field for hashtags?

```
table: activity field: hashtags  value: AftonAlps,Breckenridge,Copper,Vail
```

```
table: activity field: hashtag1 value: AftonAlps
field: hashtag2 value: Breckenridge
field: hashtag3 value: Copper
field: hashtag4 value: Vail
field: hashtag5 value: 
field: hashtag6 value: 
```

```
table: activity field: hashats value:hash_id
table: hashtags field: hash_id
```



**Deciding what JSON attributes to convert: JSON tweet templates**

One fundamental challenge of converting social media data, regardless of formats, is the sheer size of the datasets. A single activity may consist of over 150 attributes. Even a dataset with 1,000,000 social activities, a relatively small dataset, then results in 150,000,000 attributes names and values.  


Going back to the hashtag example, here is the complete hashtag metadata provided:

```
{"twitter_entities": {
    "hashtags": [
      {
        "text": "AlftonAlps",
        "indices": [
          93,
          106
        ]
      }
    ]
  }
}
  
```





*Example Use-case: Exploring New Years Eve*

The process of converting tweets from JSON to CSV was much more complicated than anticipated. To help illustrate the path taken for this project, we'll tell a short data story. We'll survey a 24-hour period looking for Twitter signals around New Years Eve 2014 in three different parts of the world.  




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
