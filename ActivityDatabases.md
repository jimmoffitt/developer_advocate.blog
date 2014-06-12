##Storing Social Media Data in Relational Databases

###Introduction

There are many ways to store social media data. Consumers of social media data often decide to store the data in relational database. There are several key questions to ponder as you design your database schema:

* What metadata is provided and what of it is needed for analysis and research? 
* How long will the data be stored? Will the stored data be from a moving windows of time, say 90 days, or will the database continually be added to?
* Many attributes of social data are delivered as arrays with variable lengths. For Twitter data, these include hashtags, user mentions, and URLs. Given that database schemas are structurally static, how will these arrays be stored?
* Many attributes of social data do not change very often. For example, tweet data includes metadata about the author that rarely changes, such as their display name, account ID, profile location, and bio description. Other attributes change slowly such as follower, friend, and favorite counts. Does your use-case involve tracking these changes, and what trade-offs are there for doing so?   
 
In this article we'll explore these fundamental decisions, discuss options when designing your database schema, and provide some example schemas for getting started.

###Getting started. 
#####[New to relational database schema design](https://www.google.com/webhp?sourceid=chrome-instant&ion=1&espv=2&ie=UTF-8#q=relational%20database%20schema%20design)? 

At the highest level: 
* Database schemas consist of tables that are made up of one of more rows.  
* Table rows are made up of one or more fields.
* Field names and types describe the table schema.
* Database schemas also involve details such as primary keys, foreign keys, and indexes. 
     * Primary keys are made up of individual or groups of fields.
     * Primary keys provide a mechanism to enforce uniqueness of your table contents by preventing duplicates.
* Indexes are helpers for searching and selecting data.  
   * When you define an index you are asking your database engine to essentially pre-sort your data, making it faster to search. 
   * Indexes come with overhead since they consist of copies of your data and thus make your database size larger. Therefore they should be created based on the types of queries your use-case needs to support. 
* Foreign Keys define how one table relates to another table. Foreign keys are made up of one or more fields in one table that uniquely identifies a row in another table. 

The following discussion will focus mainly on suggested options for specifying tables and fields, and less on recommendations for defining indexes. Creation of indexes should be driven by your data retrieval patterns.
 
The examples below are based on storing Twitter data in a database. If you are working with data from another social network, these examples will still illustrate the type of design considerations and potential techniques for storing your data. If you are storing data from multiple sources it is likely that there are some fundatmental metadata common to all such as posted time, activity IDs and author IDs. Other important details will be source-specific, such as the type of activity (post or 'like') and the length of the activity "body" (short tweet or long blog post). While it is certainly possible to store a mix of sources in a single table, there are many advantages to storing sources in their own unique tables. 

###What Metadata do you need to store?

When storing activity data in a database, you are essentially passing the data through a transform where you cherry-pick the data you care about. Inserting social media data into a database provides an opportunity to filter the incoming data, explicitly storing the data you want to keep, and ignoring the data you do not want. 

Every tweet arrives with a large set of supporting metadata. This set can contain well over 150 attributes that provide information about the user that posted the tweet, any available geographic information, and other information such as lists of hashtags and user mentions included in the tweet message. 

```
<embed a sample tweet>
    hey @lbjonz I am daydreaming of all things #snow: #skiing #boarding #caves
</embed>
```

The entire JSON associated with the above tweet is [HERE]. For a look of all the potential metadata that can be provided see [HERE](https://github.com/jimmoffitt/pt-dm/blob/master/schema/tweet_everything.json).

Given your particular use-case you may only need a subset of this supporting metadata and decide not to store every piece of data provided. For example, the standard Twitter metadata includes the numeric character position of hashtags in a tweet message. You may decide that you do not need this information, and therefore can omit those details from your database schema. 

To filter out such data means simply that you do not have a field in your database to store it, and when parsing the tweet payload you simply ignore the attribute.

--------------------------

```
sidebar here (with below contents)
```

*How can I protect myself from later realizing that there are key metadata that I haven't been storing?* 

When working with Twitter data for some [flood-related blog posts](http://blog.gnip.com/tweeting-in-the-rain/) I was not originally storing follower counts in its own field. Later I wanted to study how follower counts for public agencies increased during and after flood events. Luckily, I had two options to provide that opportunity. 

First, I was working with [Historical PowerTrack](http://support.gnip.com/apis/historical_api/) data and I had saved the time-series data files that that product generates. It is considered a best-practice to store the complete 'raw' data. Complete JSON payloads can be inserted into a NoSQL system, such as MongoDB, or as flat-files. Doing this provides a redundant datastore in case you have database problems. Also, assuming you are not storing every metadata attribute, this provides an 'insurance policy' against discovering that there is metadata needed for analysis that have not been part of your database schema. 

Second, when I designed the database schema for the flood project I decided to store each JSON payload as a separate field in my "activity" table. For this project I knew going in that I was going to have less than 500,000 activities so the overhead from this extra storage seemed worth it. For many use cases, this type of redundant storage may not scale very well. 

It should be noted that regardless of the method here, it can be a bit painful to parse and load the "missing" JSON data a second time. It is highly recommended to learn from my mistake and get your schema specified correctly the first time!

--------------------------

###Storing Metadata Arrays
Twitter data is dynamic in nature, and includes several types of metadata that are in arrays of variable length. For example, tweets can consist of multiple hashtags, urls, user mentions, and soon via the firehose, multiple photographs.  JSON readily supports arrays of data, while schemas are static in nature.  The concept of having a database field 'grow' to store dynamic array lengths of data does not exist.  To manage this strutural incongruity, there are three basic schema design strategies:

1) Store delimited lists in a single field.

* Pros: Most compact schema design, simple queries to select data (no joins).
* Cons: software inserting data needs to create the delimited list of metadata. 
      software selecting data from database needs to have special logic to 'unpack' the delimited lists. 

2) Create a set of fields to hold multiple instances.
For example, hashtags could be stored in a set of fields such as hashtag_1, hashtag_2, hashtag_3, hashtag_4. For short-content sources like Twitter, limited to 140 characters, there is a good chance that there is a reasonable upper-limit on the number of items you need to support.

* Pros: (can't think of any!)
* Cons: uglier queries, sparse contents (lots of empty fields), hardcodes the number of intenties you can process at the schema level. 

3) Create separate tables to hold multiple instances.
This method relies on have separate tables to store multiple items. For example you could have 'hashtags' and 'mentions' tables that are tied to your 'activity' table by the activity ID. This design provides for the dynamic and '3-d' nature of JSON objects.
* Pros: Efficiently models the dynamic nature of JSON objects where there is a variable list of metadata attributes. 
* Cons: (any?)

 
-----------
###Tracking Select Time-series Changes 

Many use-cases benefit from tracking changes to certain metadata that changes over time. For example, perhaps you want to track the amount of followers an account has during a on-line campaign. One way to do this is to store this type of data at the activity level so things such as actor metadata are stored along with each tweet the actor posts. 





##Some Example Schemas

(below are some examples of 'creation' scipts. )




###Generating Schemas with Ruby ActiveRecord 

* This schema mandates that metadata arrays (twitter entities, matching rules, etc) be flattened into a delimited field. An alternative is to have associated tables (such as a hashtag table). How do you guys store metadata arrays?


```
ActiveRecord::Schema.define(:version => 20140517212018) do

  create_table "activities", :force => true do |t|
    t.string 'native_id'
    t.datetime 'posted_at'
    t.text 'content'
    t.string 'body'
    t.integer 'actor_id'     #--> actor table id
    t.string 'verb'
    t.integer 'repost_of'
    t.string 'lang'              
    t.string 'pub_lang'   #--> twitter_lang 

    #These are flattened arrays, comma delimited (?)
    t.string 'hash_tags'
    t.string 'mentions'
    t.text 'urls'               #Expanded URLs when available
    t.string 'media'
    t.text 'rule_values'   
    t.text 'rule_tags'        
                            
    #Activity geo details
    t.string 'place'
    t.string 'country_code'
    t.float 'long'
    t.float 'lat'

    #Activities may be coming from different products/publishers
    t.integer 'stream_id' 
    t.string 'publisher'   

    t.datetime 'created_at'
    t.datetime 'updated_at'
  end

create_table "actors", :force => true do |t|
    t.string 'native_id'
    t.string 'bio'
    t.integer 'followers_count'
    t.integer 'friends_count'
    t.integer 'statuses_count'
    t.integer 'klout_score'
    t.text 'topics'   #klout topics   #flattened array
    t.string 'lang'
    t.string 'time_zone'
    t.integer 'utc_offset'
    t.datetime 'posted_at'

    #Actor geo metadata
    t.string 'location'
    #These really are flattened arrays, but currently will only have one item.
    t.string 'profile_geo_name'
    t.float 'profile_geo_long'
    t.float 'profile_geo_lat'
    t.string 'profile_geo_country_code'
    t.string 'profile_geo_region'
    t.string 'profile_geo_subregion'
    t.string 'profile_geo_locality'
    
    t.datetime 'created_at'
    t.datetime 'updated_at'
  end
```


###Generating Schemas with MySQL Scripts 


```
DROP TABLE IF EXISTS activities;
CREATE TABLE `activities` (
    `act_table_id` INT(16) NOT NULL AUTO_INCREMENT                      # arb integer
    , `activityId` BIGINT UNSIGNED NOT NULL DEFAULT 0                   # snowflake 
    , `powertrack_rule` TEXT NOT NULL DEFAULT ''                        # Search query that returned this data
    , `act_created_at` DATETIME NOT NULL DEFAULT '0000-00-00 00:00:00'  # timestamp of table load 
    , `postedTime` DATETIME NOT NULL DEFAULT '0000-00-00 00:00:00'      # activity postedTime
    , `verb` CHAR(16) DEFAULT NULL                                      # post/share/compliance 
    , `actor_id` INT(16) UNSIGNED NOT NULL DEFAULT 0                    # numerical id  
    , `body` TEXT DEFAULT NULL                                          # activity body text (whitespace padded) 
    , `twitter_lang` CHAR(8) DEFAULT NULL                               # tweet body lang (tw) 
    , `gnip_lang` CHAR(8) DEFAULT NULL                                  # tweet body lang (enrichment)
    , `link` CHAR(255) DEFAULT NULL                                     # www activity location url 
    , `generator_displayName` CHAR(255) DEFAULT NULL                    # service / platform
    , `geo_lat` DECIMAL(11,8) DEFAULT NULL                              # geo-tagged coord (points only)
    , `geo_lon` DECIMAL(11,8) DEFAULT NULL                              # (slightly absurd scale & precision)
    , `entities_symbol1` CHAR(255) DEFAULT NULL                         # symbol (e.g. cashtag) 
    , `entities_symbol2` CHAR(255) DEFAULT NULL                         # symbol (e.g. cashtag)
    , `entities_symbol3` CHAR(255) DEFAULT NULL                         # symbol (e.g. cashtag)
    , `entities_symbol4` CHAR(255) DEFAULT NULL                         # symbol (e.g. cashtag)
    , `entities_symbol5` CHAR(255) DEFAULT NULL                         # symbol (e.g. cashtag)
    , `entities_mention1_id` INT(16) UNSIGNED DEFAULT NULL                       # mention (user id) 
    , `entities_mention1_name` CHAR(20) DEFAULT NULL                    # mention (display name) 
    , `entities_mention2_id` INT(16) UNSIGNED DEFAULT NULL                       # mention (user id) 
    , `entities_mention2_name` CHAR(20) DEFAULT NULL                    # mention (display name) 
    , `entities_mention3_id` INT(16) UNSIGNED DEFAULT NULL                       # mention (user id) 
    , `entities_mention3_name` CHAR(20) DEFAULT NULL                    # mention (display name) 
    , `entities_mention4_id` INT(16) UNSIGNED DEFAULT NULL                       # mention (user id) 
    , `entities_mention4_name` CHAR(20) DEFAULT NULL                    # mention (display name) 
    , `entities_mention5_id` INT(16) UNSIGNED DEFAULT NULL                       # mention (user id) 
    , `entities_mention5_name` CHAR(20) DEFAULT NULL                    # mention (display name) 
    , `entities_url1_short` CHAR(64) DEFAULT NULL                       # url (t.co)
    , `entities_url1_expanded` TEXT DEFAULT NULL                   # url (unwound)
    , `entities_url2_short` CHAR(64) DEFAULT NULL                       # url (t.co)
    , `entities_url2_expanded` TEXT DEFAULT NULL                   # url (unwound)
    , `entities_url3_short` CHAR(64) DEFAULT NULL                       # url (t.co)
    , `entities_url3_expanded` TEXT DEFAULT NULL                   # url (unwound)
    , `entities_url4_short` CHAR(64) DEFAULT NULL                       # url (t.co)
    , `entities_url4_expanded` TEXT DEFAULT NULL                   # url (unwound)
    , `entities_url5_short` CHAR(64) DEFAULT NULL                       # url (t.co)
    , `entities_url5_expanded` TEXT DEFAULT NULL                   # url (unwound)
    , `entities_media1_id` BIGINT DEFAULT NULL                         # media (photo) id
    , `entities_media1_url` TEXT DEFAULT NULL                      # media (photo) expanded url
    , `entities_media2_id` BIGINT DEFAULT NULL                         # media (photo) id
    , `entities_media2_url` TEXT DEFAULT NULL                      # media (photo) expanded url
    , `entities_media3_id` BIGINT DEFAULT NULL                         # media (photo) id
    , `entities_media3_url` TEXT DEFAULT NULL                      # media (photo) expanded url
    , `entities_media4_id` BIGINT DEFAULT NULL                         # media (photo) id
    , `entities_media4_url` TEXT DEFAULT NULL                      # media (photo) expanded url
    , `entities_media5_id` BIGINT DEFAULT NULL                         # media (photo) id
    , `entities_media5_url` TEXT DEFAULT NULL                      # media (photo) expanded url
    , PRIMARY KEY (`activityId`)
) 
ENGINE=MyISAM AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci
;
```


