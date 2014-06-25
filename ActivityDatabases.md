[] add notes about schemas here are twitter specific
[] if mixing publishers/products may need to add fields for tracking those.
[] Link to ActiveRecord types
[] Link to MySQL types  http://dev.mysql.com/doc/refman/5.0/en/numeric-type-overview.html
[] Unified field comments for all "one table" contents


##Storing Social Media Data in Relational Databases

###Introduction

There are many ways to store social media data. Consumers of social media data often decide to store the data in relational database. There are several key questions to ponder as you design your database schema:

* What metadata is provided and what of it is needed for analysis and research? 
* How long will the data be stored? Will the stored data be from a moving windows of time, say 90 days, or will the database continually be added to?
* Many attributes of social data are delivered as arrays with variable lengths. For Twitter data, these include hashtags, user mentions, and URLs. Given that database schemas are structurally static, how will these arrays be stored?
* Many attributes of social data do not change very often. For example, tweet data includes metadata about the author that rarely changes, such as their display name, account ID, profile location, and bio description. Other attributes change slowly such as follower, friend, and favorite counts. Does your use-case involve tracking these changes, and what trade-offs are there for doing so?   
 
In this article we'll explore these fundamental decisions, discuss options when designing your database schema, and provide some example schemas for getting started.

###Getting started 
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
    hey @lbjonz on this summer weekend I am daydreaming of all things #snow: #skiing #boarding #caves
</embed>
```

<blockquote class="twitter-tweet" lang="en"><p>hey <a href="https://twitter.com/lbjonz">@lbjonz</a> on this summer weekend I am daydreaming of all things <a href="https://twitter.com/search?q=%23snow&amp;src=hash">#snow</a>: <a href="https://twitter.com/search?q=%23skiing&amp;src=hash">#skiing</a> <a href="https://twitter.com/search?q=%23boarding&amp;src=hash">#boarding</a> <a href="https://twitter.com/search?q=%23caves&amp;src=hash">#caves</a></p>&mdash; Jim Moffitt (@snowman) <a href="https://twitter.com/snowman/statuses/480209697199243264">June 21, 2014</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

The entire JSON associated with the above tweet is [HERE](https://github.com/jimmoffitt/pt-dm/blob/master/schema/hashtags.json). For a look of all the potential metadata that can be provided see [HERE](https://github.com/jimmoffitt/pt-dm/blob/master/schema/tweet_everything.json).

Given your particular use-case you may only need a subset of this supporting metadata and decide not to store every piece of data provided. For example, the standard Twitter metadata includes the numeric character position of hashtags in a tweet message. You may decide that you do not need this information, and therefore can omit those details from your database schema. 

To filter out such data means simply that you do not have a field in your database to store it, and when parsing the tweet payload you simply ignore the attribute.

--------------------------

```
sidebar here (with below contents)
```

*How can I protect myself from later realizing that there are key metadata that I haven't been storing?* 

When you design your database schema you are explicitly saving selected data, and anything not selected is ignored and not available for future use. Given this, it pays to carefully identify what data you need the first time.

When working with Twitter data for some [flood-related blog posts](http://blog.gnip.com/tweeting-in-the-rain/) I was not originally storing follower counts in its own field. Later I wanted to study how follower counts for public agencies increased during and after flood events. Luckily, I had two options to provide that opportunity. 

First, I was working with [Historical PowerTrack](http://support.gnip.com/apis/historical_api/) data and I had saved the time-series data files that that product generates. It is considered a best-practice to store the complete 'raw' data. Complete JSON payloads can be inserted into a NoSQL system, such as MongoDB, or as flat-files. Doing this provides a redundant datastore in case you have database problems. Also, assuming you are not storing every metadata attribute, this provides an 'insurance policy' against discovering that there is metadata needed for analysis that have not been part of your database schema. 

Second, when I designed the database schema for the flood project I decided to store each JSON payload as a separate field in my "activity" table. For this project I knew going in that I was going to have less than 500,000 activities so the overhead from this extra storage seemed worth it. For many use cases, this type of redundant storage may not scale very well. 

It should be noted that regardless of the method here, it can be a bit painful to parse and load the "missing" JSON data a second time. It is highly recommended to learn from my mistake and get your schema specified correctly the first time!

--------------------------

###Storing Metadata Arrays
Twitter data is dynamic in nature, and includes several types of metadata that are in arrays of variable length. For example, tweets can consist of multiple hashtags, urls, user mentions, and soon via the firehose, multiple photographs. For example the tweet above contains four hashtags: #snow, #skiing, #boarding, #caves. 

JSON readily supports arrays of data, while schemas are static in nature.  The concept of having a database field 'grow' to store dynamic array lengths of data does not exist.  To manage this strutural incongruity, below are three basic schema design strategies. For each example some Ruby-based pseudo-code is provided. These code examples illustrate SQL queries and loading the hashtags into an array. 

#### 1) Store delimited lists in a single field:

|  id  | hashtags                  	|  
|---------------------	| ---------|
|480209697199243264 | snow, skiing, boarding, caves |

One advantage of this schema design is that it results in a simple schema, enabling very simple SQL queries to retreive the data. 

```
SELECT hashtags FROM activities WHERE id = 480209697199243264;
```

Client code needs to 'split' the field contents using the (mutually agreed on) delimiter and then iterate through the results:

```
#SQL query to select a single set of hashtags for a specified tweet ID.
query = "SELECT hashtags FROM activities WHERE id = #(activity_id};"

hashtags = Array.new #Will load hashtags into an array.

delimiter = ',' #Need to know how hashtags are delimited. Could have self-discovering logic here.

result_set = db.execute(query)

result_set.each do |row|  
   row.each do |key, value|  #Our query specified a single field, getting back field name, value...
      hashtags_delimited << value 
   end
end

hashtags = hashtags_delimited.split(delimiter) #The joys of Ruby (and Python).
```

---------------------------------------
#### 2) Create a set of fields to hold multiple instances:

| id  | hashtag_1  | hashtag_2   | hashtag_3  | hashtag_4  | hashtag_5  | 
|----------|----------|------------|------------|------------|----------|
|480209697199243264 |  snow | skiing | boarding | caves        |            | 

With this method hashtags are stored in a set of fields such as hashtag_1, hashtag_2, hashtag_3, hashtag_4, and hashtag_5. For short-content sources like Twitter, limited to 140 characters, there is a good chance that there is a reasonable upper-limit on the number of items you need to support.

One disadvantage with this method is that you are likely to end up with a lot of empty fields since most tweets have just one or two hashtags. Another is that the design 'hard-codes' the number of entities you can store, so you need to decide how many to support. Yet another disadvantage is the SQL you need to write to process these multiple fields, where the client code and its query also hard-codes an explicit number of metadata items: 

```
SELECT hashtag_1, hashtag_2, hashtag_3, hashtag_4, hashtag_5 FROM activities WHERE id = 480209697199243264;
```

Other than the query being completely coupled to the schema details, the client-side code is much the same although it does not need any delimiter metadata.

```
   #SQL query to select a single set of hashtags for a specified tweet ID.
   query = "SELECT hashtag_1, hashtag_2, hashtag_3, hashtag_4, hashtag_5 FROM activities WHERE id = #(activity_id};";)"

   hashtags = Array.new #Will load hashtags into an array.

   result_set = db.execute(query)

   result_set.each do |row|  
      row.each do |k, v|
         hashtags << v #Just grab hashtag values, can ignore "hashtags" key (field name).
      end
   end
```   

---------------------------------------
####3) Create separate tables to hold multiple instances:

Activity table entry:

|    id    |                body                     |
|----------|-----------------------------------------|
|480209697199243264 | hey @lbjonz on this summer weekend I am daydreaming of all things #snow: #skiing #boarding #caves |

Hashtag table entries:

| id |    activity_id     |  hashtag  |
|----|--------------------|-----------|
| 1  | 480209697199243264 |  snow     |
| 2  | 480209697199243264 |  skiing   |
| 3  | 480209697199243264 |  boarding |
| 4  | 480209697199243264 |  caves    |

This method relies on having a separate table to store hashtags, with each row containing a single hashtag. So with our example tweet, there are four entries in this table, with the activity ID being the unique link (foreign key) between the two tables. 
```
SELECT ht.* FROM hashtags ht, activities a
WHERE ht.activity_id = a.id
AND a.id = 480209697199243264;
```
This design readily handles the dynamic '3-d' nature of JSON objects. Indeed, one advantage of this design is that there are not a predetermined number of hashtag fields for each activity and instead the hashtag table dynamically stores the array items as needed.

```
   #SQL query to select a single set of hashtags for a specified tweet ID.
   query = "SELECT ht.* FROM hashtags ht, activities a WHERE ht.activity_id = a.id AND a.id = #(activity_id};"

   hashtags = Array.new #Will load hashtags into an array.

   result_set = db.execute(query)

   result_set.each do |row| #Will get one row for each entry in hashtags table... 
      row.each do |k, v|
         hashtags << v
      end
   end
```   
 
-----------
###Tracking Select Time-series Changes 

Many use-cases benefit from tracking changes to certain metadata that changes over time. For example, perhaps you want to track the amount of followers an account has during a on-line campaign. One way to do this is to store this type of data at the activity level so things such as actor metadata are stored along with each tweet the actor posts. The disadvantage of storing all data at the activity level is that much of this data will be static, so significant storage space is spent on redundant data. However, the required SQL for retrieving data is simple, and client-side code remains simple. 

Another strategy is to segregate the metadata into two groups: attributes you want to track over time, and others that you only need to store one value for. With this design the more dynamic data is stored either at the activity level, or in a separate "dynamic" table, with more static data being written to another "static" table.

In the example schemas presented below, the "user_static" table illustrates this design. 

##Some Example Schemas

Below are some example schemas that provide a starting place for specifying your database schema. We start with a single-table schema that represents the most simple option. Then we present schema examples comprised of separate tables for metadata arrays (such as hashtags) and storing static metadata in a separate table.

We present two types of scripts to generate the example schemas in a MySQL database. The first type is based on the Ruby on Rails ActiveRecord framework, and the second type can be used directly with the MySQL database engine.

###Single table

As discussed above, this design has the disadvantage of ineffeciently storing redundant data, but it should be adequate for many use-cases such as datasets from Historical PowerTrack jobs with a finite amount of data.

The following exmample illustrate the most basic schema, where all metadata is stored at the activity level. This design has an disadvantage of being less efficent with respect to (mostly) static metadata.


####Creating with Ruby ActiveRecord





####MySQL creation script









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
    
    ADD ALL THE REDUNDANT ACTOR DATA!!!!!!!!!!!!!
  end
```

Another option is to store certain metadata in another table...

```
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

Here is a schema that segregates metadata into separate "static' and "dynamic" tables:


```
actor.static


```

```
actor.dynamic

```












###Generating Schemas with MySQL Scripts 

Here are some example SQL scripts for creating schemas directly with the MySQL engine:

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
    , KEY (`act_table_id`)
    , INDEX `postedTime_idx` (`postedTime`)
    , INDEX `verb_idx` (`verb`)
    , INDEX `actor_id_idx` (`actor_id`)
    , FULLTEXT INDEX `body_idx` (`body`)                                # will make load/insert slower 
    , INDEX `twitter_lang_idx` (`twitter_lang`)
    , INDEX `gnip_lang_idx` (`gnip_lang`)
    , INDEX `link_idx` (`link`)
    , INDEX `generator_displayName_idx` (`generator_displayName`)
    , INDEX `geo_lat_idx` (`geo_lat`)
    , INDEX `geo_lon_idx` (`geo_lon`)
    , INDEX `symbol1_idx` (`entities_symbol1`)
    , INDEX `symbol2_idx` (`entities_symbol2`)
    , INDEX `symbol3_idx` (`entities_symbol3`)
    , INDEX `symbol4_idx` (`entities_symbol4`)
    , INDEX `symbol5_idx` (`entities_symbol5`)
    , INDEX `mention1_id_idx` (`entities_mention1_id`)
    , INDEX `mention1_name_idx` (`entities_mention1_name`)
    , INDEX `mention2_id_idx` (`entities_mention2_id`)
    , INDEX `mention2_name_idx` (`entities_mention2_name`)
    , INDEX `mention3_id_idx` (`entities_mention3_id`)
    , INDEX `mention3_name_idx` (`entities_mention3_name`)
    , INDEX `mention4_id_idx` (`entities_mention4_id`)
    , INDEX `mention4_name_idx` (`entities_mention4_name`)
    , INDEX `mention5_id_idx` (`entities_mention5_id`)
    , INDEX `mention5_name_idx` (`entities_mention5_name`)
    , INDEX `url1_short_idx` (`entities_url1_short`)
    , FULLTEXT INDEX `url1_expanded_idx` (`entities_url1_expanded`)
    , INDEX `url2_short_idx` (`entities_url2_short`)
    , FULLTEXT INDEX `url2_expanded_idx` (`entities_url2_expanded`)
    , INDEX `url3_short_idx` (`entities_url3_short`)
    , FULLTEXT INDEX `url3_expanded_idx` (`entities_url3_expanded`)
    , INDEX `url4_short_idx` (`entities_url4_short`)
    , FULLTEXT INDEX `url4_expanded_idx` (`entities_url4_expanded`)
    , INDEX `url5_short_idx` (`entities_url5_short`)
    , FULLTEXT INDEX `url5_expanded_idx` (`entities_url5_expanded`)
    , INDEX `media1_id_idx` (`entities_media1_id`)
    , FULLTEXT INDEX `media1_url_idx` (`entities_media1_url`)
    , INDEX `media2_id_idx` (`entities_media2_id`)
    , FULLTEXT INDEX `media2_url_idx` (`entities_media2_url`)
    , INDEX `media3_id_idx` (`entities_media3_id`)
    , FULLTEXT INDEX `media3_url_idx` (`entities_media3_url`)
    , INDEX `media4_id_idx` (`entities_media4_id`)
    , FULLTEXT INDEX `media4_url_idx` (`entities_media4_url`)
    , INDEX `media5_id_idx` (`entities_media5_id`)
    , FULLTEXT INDEX `media5_url_idx` (`entities_media5_url`)
) 
ENGINE=MyISAM AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci
;


DROP TABLE IF EXISTS hashtags;
CREATE TABLE `hashtags` (
    `hashtag_table_id` INT(16) NOT NULL AUTO_INCREMENT          # arb integer
    , `activityId` BIGINT UNSIGNED NOT NULL DEFAULT 0           # tweet snowflake  
    , `text` CHAR(255) DEFAULT NULL                             # hashtag text 
    , PRIMARY KEY (`activityId`, `text`)
    , KEY (`hashtag_table_id`)
    , INDEX `activityId_idx` (`activityId`)
    , INDEX `text_idx` (`text`)
) 
ENGINE=MyISAM AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci
;

```





```
# this table maintains the user fields which do not change as often. 
#   when a new record is observed, the fields are replaced and the 
#   updated_at field is updated accordingly.
DROP TABLE IF EXISTS users_static;
CREATE TABLE `users_static` (
    `us_table_id` INT(16) NOT NULL AUTO_INCREMENT                       	# arb integer
    , `us_created_at` DATETIME NOT NULL DEFAULT '0000-00-00 00:00:00'   	# timestamp of table load 
    , `actor_id` INT(16) UNSIGNED NOT NULL DEFAULT 0                    	# numerical id - doesnt change 
    , `activityId` BIGINT UNSIGNED NOT NULL DEFAULT 0                   	# activity snowflake of last update 
    , `us_updated_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP 
    , `user_postedTime` DATETIME NOT NULL DEFAULT '0000-00-00 00:00:00' 	# user creation date
    , `preferredUsername` CHAR(64) NOT NULL DEFAULT ''                  	# account name - can change 
    , `displayName` CHAR(64) DEFAULT NULL                               	# displayed name - can change 
    , `link` CHAR(255) DEFAULT NULL                                     	# www link to account profile 
    , `summary` TEXT DEFAULT NULL                                       	# profile bio 
    , `links1_href` TEXT DEFAULT NULL                                   	# urls in bio 
    , `links2_href` TEXT DEFAULT NULL                                   	# urls in bio 
    , `twitterTimezone` CHAR(255) DEFAULT NULL                          	# text field 
    , `utcOffset` INT(16) DEFAULT NULL                                  	# timezone offset 
    , `verified` BOOLEAN NOT NULL DEFAULT False
    , `languages` CHAR(8) DEFAULT NULL                                  	# users default language 
    , `profile_geo_lat` DECIMAL(11,8) DEFAULT NULL                      	# profile_geo 
    , `profile_geo_lon` DECIMAL(11,8) DEFAULT NULL  
    , `profile_geo_country_code` CHAR(8) DEFAULT NULL
    , `profile_geo_locality` CHAR(64) DEFAULT NULL
    , `profile_geo_region` CHAR(64) DEFAULT NULL
    , `profile_geo_subregion` CHAR(64) DEFAULT NULL
    , `profile_geo_displayName` CHAR(255) DEFAULT NULL
    , `klout_user_id` BIGINT UNSIGNED DEFAULT NULL                      	# klout user id only comes with topics 
    , PRIMARY KEY (`actor_id`)
    , KEY (`us_table_id`)
    , INDEX `user_postedTime_idx` (`user_postedTime`)
    , INDEX `preferredUsername_idx` (`preferredUsername`)
    , INDEX `displayName_idx` (`displayName`)
    , INDEX `link_idx` (`link`)
    , FULLTEXT INDEX `summary_idx` (`summary`)                          	# optional, will slow load/insert a bit
    , FULLTEXT INDEX `links1_href_idx` (`links1_href`)
    , FULLTEXT INDEX `links2_href_idx` (`links2_href`)
    , INDEX `twitterTimezone_idx` (`twitterTimezone`)
    , INDEX `verified_idx` (`verified`)
    , INDEX `languages_idx` (`languages`)
    , INDEX `profile_geo_lat_idx` (`profile_geo_lat`)
    , INDEX `profile_geo_lon_idx` (`profile_geo_lon`)
    , INDEX `profile_geo_country_code_idx` (`profile_geo_country_code`)
    , INDEX `profile_geo_locality_idx` (`profile_geo_locality`)
    , INDEX `profile_geo_region_idx` (`profile_geo_region`)
    , INDEX `profile_geo_subregion_idx` (`profile_geo_subregion`)
    , INDEX `profile_geo_displayName_idx` (`profile_geo_displayName`)
    , INDEX `klout_user_id_idx` (`klout_user_id`)
) 
#ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci
ENGINE=MyISAM AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci
;


# this table maintains the newest version of fields that are 
#   expected to change somewhat to very often. In order to have the 
#   full history of each user (one user table row per activity)
#   you can basically just combine these two user tables and build 
#   a primary key on `id` 

DROP TABLE IF EXISTS `users_dynamic`;
CREATE TABLE `users_dynamic` (
    `ud_table_id` INT(16) NOT NULL AUTO_INCREMENT                   # arb integer
    , `actor_id` INT(16) UNSIGNED NOT NULL DEFAULT 0                # numerical id - doesnt change 
    , `activityId` BIGINT UNSIGNED NOT NULL DEFAULT 0               # activity snowflake of last update 
    , `ud_updated_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP 
    , `klout_score` INT(3) DEFAULT 0                                # should be in every activity? 
    , `klout_topic1_id` CHAR(32) DEFAULT NULL                       # numerical id from klout topic link 
    , `klout_topic1_displayName` CHAR(255) DEFAULT NULL             # description of topic (string)
    , `klout_topic2_id` CHAR(32) DEFAULT NULL                       # numerical id from klout topic link 
    , `klout_topic2_displayName` CHAR(255) DEFAULT NULL             # description of topic (string)
    , `statusesCount` INT(16) NOT NULL DEFAULT 0                    # countable things 
    , `followersCount` INT(16) NOT NULL DEFAULT 0 
    , `friendsCount` INT(16) NOT NULL DEFAULT 0 
    , `listedCount` INT(16) NOT NULL DEFAULT 0 
    , PRIMARY KEY (`actor_id`, `activityId`)
    , KEY (`ud_table_id`)
    , INDEX `klout_score_idx` (`klout_score`)
    , INDEX `klout_topic1_id_idx` (`klout_topic1_id`)
    , INDEX `klout_topic1_displayName_idx` (`klout_topic1_displayName`)
    , INDEX `klout_topic2_id_idx` (`klout_topic2_id`)
    , INDEX `klout_topic2_displayName_idx` (`klout_topic2_displayName`)
    , INDEX `statusesCount_idx` (`statusesCount`)
    , INDEX `followersCount_idx` (`followersCount`)
    , INDEX `friendsCount_idx` (`friendsCount`)
    , INDEX `listedCount_idx` (`listedCount`)
) 
#ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci
ENGINE=MyISAM AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci
;
```
