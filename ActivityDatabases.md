------------------------------------------------------------------
To-dos:

* [] check pseudo-code for 'multiple field' example.
* [] Update ActiveRecord schema.rb
* [] Link to ActiveRecord types
* [] Link to MySQL types  http://dev.mysql.com/doc/refman/5.0/en/numeric-type-overview.html
* [] Generate more ActiveRecord/Model example code
   *      [] Introduce ActiveRecord fundamentals (separately)
   *      [] auto-increment ID primary key, created_at, updated_at, migrations, schema.rb 
   *      [] has_many, uniqueness, etc.
* [] Add section on "example data questions."
* [x] Add pseudo-code for accessing data.
* [x] Add link to MySQL schema creation script.
* [x] add note about schemas here are twitter specific
*      [x] if mixing publishers/products may need to add fields for tracking those.
##Storing Social Media Data in Relational Databases

###Sections/Articles:
   - Getting started - What Metadata do you need to store?
      -     What data questions to you want to explore? (started)
      -     Example questions --> queries --> schema (started)
   - Storing Metadata Arrays
   - Tracking Select Time-series Changes (started)
   - Example Schemas
   - Ruby/Rails examples (started)
   - Java examples (not started, have sample code ready)
 
###Introduction

There are many ways to store social media data, such as flat-files, NoSQL-type datastores, and relational databases. This article focuses on storing Twitter data in relational databases. There are several key questions to ponder as you design your database schema:

* What metadata is provided and what of it is needed for analysis and research? 
* How big will your database get? Are you continually filling your data store from a 24/7 stream of data? Or are you working with a historical and static dataset?
* How long will the data be stored? Will the stored data be from a moving windows of time, say 90 days, or will the database continually be added to?
* Many attributes of social data are delivered as arrays with variable lengths. For Twitter data, these include hashtags, user mentions, and URLs. Given that database schemas are structurally static, how will these arrays be stored?
* Many attributes of social data do not change very often. For example, tweet data includes metadata about the author that rarely changes, such as their display name, account ID, profile location, and bio description. Other attributes change slowly, such as follower, friend, and favorite counts. Does your use-case involve tracking these changes, and what trade-offs are there for doing so?   
 
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


###What activity metadata do you need to store?

When storing activity data (in this case tweets) in a database, you are essentially passing the data through a transform where you cherry-pick the data you care about. Inserting social media data into a database provides an opportunity to filter the incoming data, explicitly storing the data you want to keep, and ignoring the data you do not want. 

Every tweet arrives with a large set of supporting metadata. This set can contain well over 150 attributes that provide information about the user that posted the tweet, any available geographic information, and other information such as lists of hashtags and user mentions included in the tweet message. 

<blockquote class="twitter-tweet" lang="en"><p>hey <a href="https://twitter.com/lbjonz">@lbjonz</a> on this summer weekend I am daydreaming of all things <a href="https://twitter.com/search?q=%23snow&amp;src=hash">#snow</a>: <a href="https://twitter.com/search?q=%23skiing&amp;src=hash">#skiing</a> <a href="https://twitter.com/search?q=%23boarding&amp;src=hash">#boarding</a> <a href="https://twitter.com/search?q=%23caves&amp;src=hash">#caves</a></p>&mdash; Jim Moffitt (@snowman) <a href="https://twitter.com/snowman/statuses/480209697199243264">June 21, 2014</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

The entire JSON associated with the above tweet is [HERE](https://github.com/jimmoffitt/pt-dm/blob/master/schema/hashtags.json). For a look of all the potential metadata that can be provided see [HERE](https://github.com/jimmoffitt/pt-dm/blob/master/schema/tweet_everything.json).

Given your particular use-case you may only need a subset of this supporting metadata and decide not to store every piece of data provided. For example, the standard Twitter metadata includes the numeric character position of hashtags in a tweet message. You may decide that you do not need this information, and therefore can omit those details from your database schema. 

To filter out such data means simply that you do not have a field in your database to store it, and when parsing the tweet payload you simply ignore the attribute.


###An Example Use-Case

In the end database schemas are driven by the type of questions you want to explore with social data. Given my background and interest in flood-warning systems I wanted to explore the role Twitter data played in the 2013 Boulder Flood -- an historic rain and flood event that occurred September 12-15 and had lasting affects across the Boulder region.  The questions I wanted to explore were:

* How did the Twitter signal track with local rain gauge data?
     * [Previous analysis of most less intense events revealed a strong signal](http://blog.gnip.com/tweeting-in-the-rain/). How would the comparison of rain gauge data and Twitter data differ for a 1000-year flood?
* How did the Twitter signal track with local stage gauge data? 
     * How did that signal attenuate as the flood waters moved downstream?  
* How many unique users posted during the event?
* What was the precentage of tweets were geo-tagged during the event?
* How many real-time photos and video were coming out on Twitter during the event?
* How did the local media cover the event?
* How did the followers of local agencies change during and after the 2013 flood?
* What are the social media lessons learned from this event?

So these types of questions guided the design of the database schema at the heart of my look at the 2013 Boulder flood.


*************************************
```
sidebar here (with below contents)
```
##### *How can I protect myself from later realizing that there are key metadata that I haven't been storing?*

When you design your database schema you are explicitly saving selected data, and anything not selected is ignored and not available for future use. Given this, it pays to carefully identify what data you need the first time.

When working with Twitter data for some [flood-related blog posts](http://blog.gnip.com/tweeting-in-the-rain/) I was not originally storing follower counts in its own field. Later I wanted to study how follower counts for public agencies increased during and after flood events. Luckily, I had two options to provide that opportunity. 

First, I was working with [Historical PowerTrack](http://support.gnip.com/apis/historical_api/) data and I had saved the time-series data files that that product generates. It is considered a best-practice to store the complete 'raw' data. Complete JSON payloads can be inserted into a NoSQL system, such as MongoDB, or as flat-files. Doing this provides a redundant datastore in case you have database problems. Also, assuming you are not storing every metadata attribute, this provides an 'insurance policy' against discovering that there is metadata needed for analysis that have not been part of your database schema. 

Second, when I designed the database schema for the flood project I decided to store each JSON payload as a separate field in my "activity" table. For this project I knew going in that I was going to have less than 500,000 activities so the overhead from this extra storage seemed worth it. For many common use cases, this type of redundant storage will not scale very well. 

It should be noted that regardless of the method here, it can be a bit painful to parse and load the "missing" JSON data a second time. It is highly recommended to learn from my mistake and get your schema specified correctly the first time!

*************************************

##Storing Metadata Arrays
Twitter data is dynamic in nature, and includes several types of metadata that are in arrays of variable length. For example, tweets can consist of multiple hashtags, urls, user mentions, and photographs. For example the tweet above contains four hashtags: #snow, #skiing, #boarding, #caves. 

JSON readily supports arrays of data, while schemas are static in nature.  The concept of having a database field 'grow' to store dynamic array lengths of data does not exist in realtional databases.  To manage this strutural incongruity, below are three basic schema design strategies. For each example some pseudo-code, with a strong Ruby ascent, is provided. These code examples illustrate SQL queries and loading the hashtags into an array. 

#### 1) Store delimited lists in a single field:

|  id  | hashtags                  	|  
|---------------------	| ---------|
|480209697199243264 | snow, skiing, boarding, caves |

One advantage of this schema design is that it results in a simple schema, enabling very simple SQL queries to retreive the data. 

```
SELECT hashtags 
FROM activities 
WHERE id = 480209697199243264;
```

Client code needs to 'split' the field contents using the (mutually agreed on) delimiter and then iterate through the results:

```
#SQL query to select a single set of hashtags for a specified tweet ID.
query = "SELECT hashtags FROM activities WHERE id = #{activity_id};"
db.connect if not db.active
result_set = db.execute(query)

hashtags = Array.new #Will load hashtags into an array.
delimiter = ',' #Need to know how hashtags are delimited. Could have self-discovering logic here.

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
SELECT hashtag_1, hashtag_2, hashtag_3, hashtag_4, hashtag_5 
FROM activities 
WHERE id = 480209697199243264;
```

Other than the query being completely coupled to the schema details, the client-side code is much the same although it does not need any delimiter metadata.

```
   #SQL query to select a single set of hashtags for a specified tweet ID.
   query = "SELECT hashtag_1, hashtag_2, hashtag_3, hashtag_4, hashtag_5 FROM activities WHERE id = #{activity_id};";)"
   db.connect if not db.active
   
   hashtags = Array.new #Will load hashtags into an array.

   result_set = db.execute(query)

   result_set.each do |row|  
      row.each do |k, v|
         hashtags << v #Just grab hashtag values, ignoring "hashtags" key (field name).
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
   db.connect if not db.active
   
   hashtags = Array.new #Will load hashtags into an array.

   result_set = db.execute(query)

   result_set.each do |row| #Will get one row for each entry in hashtags table... 
      row.each do |k, v|
         hashtags << v  
      end
   end
```   
 
-----------
##Storing Dynamic and Static Metadata

While much of Twitter metadata is dynamic in nature, changing tweet-by-tweet, other metdata can change more slowly or stay completely static for long periods of time. Twitter user accounts (stored in the 'Actor' object in the Activity Stream format) provides good examples of both 'static' and 'slow' metadata. An actor's numeric ID and account creation time will never change. Their account-level language, timezone, location, display and handle names may rarely change. Meanwhile their status, followers and friends counts will surely change over time.    

Here are some example attributes that generally fall into these three categories:

* Mostly Dynamic/Active:
     * Tweet body, published time and link.
     * Hashtags, Mentions, URLs, media and other Twitter "entities".
     * Gnip matching rules and expanded URLs.
     * Tweet geographic location (if geo-tagged).
     * 

* Change more slowly:
     * User/Actor follower, friend, status counts.
     * Klout score and topics.


* Mostly Static:
      * User/Actor account ID, display and handle names, language, timezone.  
      * User/Actor Profile Location and Gnip Profile Geo enrichment.
      * Twitter provider object.
      * Tweet language (both Twitter and Gnip classifications).

How you manage and store these metadata depends on your specific use-case and its data analysis requirements. For example, perhaps you want to track the amount of followers an account has during a on-line campaign.  

Here are a few schema options to consider:

####Store all metadata at the activity level

One method is to store all metadata at the activity (tweet) level so all attributes such as actor metadata are stored along with each tweet the actor posts. While this is the most simple design, it has a fundamental disadvantage. Much of these data will be static, so significant storage space is spent on redundant data. However, the required SQL for retrieving data is simple, and client-side code remains simple. If you are working with a finite dataset, such as a "one-off" from Gnip's Historical PowerTrack product, this simple design may be adequate. However, if your dataset is continually growing you may want to condider the others below.

See [HERE](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/ActivityDatabases.md#single-table) for an example (ActiveRecord) schema for storing all tweet metadata in a single table.


####Store select dynamic and static metadata in separate tables

Another option is to segregate the metadata into two groups: attributes you want to track over time, and others that you only need to store one value for, such as the most recent value. For example, you could define a 'user_static' table that contains fields such as 'preferredUsername', 'link', 'postedTime', 'languages', and 'twitterTimeZone'. Then for fields that are more likely to change you can define a 'user_active' table that stores fields such as 'followersCount' and 'favoritesCount'.

See [HERE](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/ActivityDatabases.md#dynamic-and-static-object-attributes) for two example tables for storing static and dynamic attributes separately.


####Store dynamic metadata at activity level and static data in its own tables

With this design the more dynamic data is stored at the activity level table, with more constant data being written to a "static" table. This is sort of a hybrid design that offers some efficiencies for storing static data along with simplicity for storing dynamic data. 

This is the schema design selected for the flood project mentioned above. See [HERE] for that schema design.


##Some Example Schemas

Below are some example schemas that provide a starting place for specifying your database schema. We start with a single-table schema that represents the most simple option. Then we provide example tables for segregating Actor attributes into separate 'static' and 'active' tables. Finally, we present the schema selected for our example flood use-case, one that stores dynamic Actor attributes at the tweet level, stores more static Actor attributes in a separate Actor table, and has separate tablles for select metadata arrays such as hashtags and matching rules.

###Creating Databases

We present two types of scripts to generate the example schemas in a MySQL database. The first type is based on the Ruby on Rails ActiveRecord framework. To create these databases, the 'rake' engine can be used by pointing it to the schema.rb file with the following command:

```
rake db:schema:load
```

Another database creation method is using a MySQL 'create table' script. These scripts can be executed with the interactive MySQL client on its command-line, or with applications such as the MySQL Workbench or Sequel Pro. [HERE](script.](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/single_table_schema_mysql.md) is an example set of SQL statements for creating a single table schema.

###ActiveRecord Conventions

One of the great things about Ruby-on-Rails is its reliance on 'Convention over Configuration. Accordingly, ActiveRecord has its own conventions which should be considered when working with the schema definitions below.

First, there is a convention of every table having an "id" (auto-increment) primary key that is not explicitly shown in the 'create_table' method. Another convention is that the 'created_at' and 'updated_at' attributes are automatically added by default (and explicitly shown in the schema definition).

Also, another convention is that if there is a '*_id' field (like 'actor_id') that references the singular name of another table it is a foreign key into that separate table. This convention is the foundation of [Rails Associations](http://guides.rubyonrails.org/association_basics.html) that provides a lot of power and convenience if you are building a Rails application. For example, consider a schema with Activities and Actors tables. Both these tables will contain a 'id' primary key and that by convention serves as a foreign key when joining tables. If you are storing tweet authors in an Actor table, the Activity table will contain an actor_id field used to match the appropriate entry in the Actor table. 

Finally, you will certainly want to retain the 'native' user and tweet IDs (at least the numeric section, dropping the string versioning metadata), or the numeric IDs provided by Twitter. Accordingly, you may be storing two IDs, the 'native' versions along with the auto-increment IDs provided by ActiveRecord.

[OK, need some Rails advice here... only need ActiveRecord ids if building Rails code. otherwise, not]

Activity table entry:

|    id      |   native_id    |   actor_id          |     actor_native_id    |        body               |
|----------|--------------|---------|-------------|------------------|
|    1000        | 480209697199243264 |    123   |     17200003       |    hey @lbjonz on this summer weekend I am daydreaming of all things #snow: #skiing #boarding #caves |

Actor table entry:

| id |    native_id     |  handle  |
|-----------|--------------------|-----------|
| 123  | 17200003    |  snowman     |


So to join these two tables you can rely on the default 'id' foreign key:

```
Actor.id = Activity.actor_id
```

And write some SQL such as:

```
SELECT * 
FROM activities a, actors act
WHERE a.actor_id = act.id
AND act.handle = 'snowman';
```







###Single table

As discussed above, this design has the disadvantage of inefficiently storing redundant data, but it should be adequate for many use-cases such as datasets from Historical PowerTrack jobs with a finite amount of data.

The following example illustrates the most basic schema, where all metadata is stored at the activity level. This design has an disadvantage of being less efficient with respect to (mostly) static metadata. Also, metadata arrays, such as hashtags and mentions, are stored in a single field.

[Creating with Ruby ActiveRecord](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/single_table_schema.md)


```
ActiveRecord::Schema.define(:version => 20140624212018) do

  create_table "activities", :force => true do |t|

    #t.integer 'id'           #auto-increment field that other tables' activity_id foreign keys refer to.      
    t.string 'tweet_id'
    t.datetime 'posted_at'
    t.text 'payload'          #Entire content of JSON activity...? 
    t.string 'body'
    t.string 'verb'
    t.integer 'repost_of'
    t.string 'gnip_lang'            
    t.string 'twitter_lang'
    t.string 'generator'
    t.string 'link'

    #These are flattened arrays, comma delimited (?)
    t.string 'hash_tags'
    t.string 'mentions'
    t.text 'urls'         #Expanded URLs when available.
    t.string 'media'
    t.text 'rule_values'  #Gnip PowerTrack matching rules. 
    t.text 'rule_tags'        

    #Activity geo details - Geo-tagged tweets only.
    t.string 'place'
    t.string 'country_code'
    t.float 'long'
    t.float 'lat'
    t.float 'long_box' #If storing place bounding box.
    t.float 'lat_box'  

    #Actor metadata
    t.integer 'actor_native_id'  
    t.string 'preferredUsername'
    t.string 'displayName'
    t.string 'actor_link'
    t.string 'bio'
    t.integer 'followers_count'
    t.integer 'friends_count'
    t.integer 'statuses_count'
    t.integer 'klout_score'
    t.text 'klout_topics'   #klout topics #flattened array.
    t.string 'actor_lang'
    t.string 'time_zone'
    t.integer 'utc_offset'
    t.datetime 'posted_at'

    #Actor geo metadata
    t.string 'actor_location' #Twitter Profile location.
    #Only needed if Gnip Profile Geo enabled.
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

#### Tables for Separating Static and Dynamic Metadata

Below are two example table definitions that split Actor attributes into separate 'static' and 'active' tables. 

```
create_table "actors_static", :force => true do |t|
    t.string 'native_id'
    t.string 'handle'
    t.string 'display_name'
    t.string 'bio'
    t.string 'lang'
    t.string 'time_zone'
    t.integer 'utc_offset'
    t.datetime 'posted_at'
    
    #Actor geo metadata
    t.string 'location'
    #These really are flattened arrays, but currently will only have one item.
    t.string 'profile_geo_name'
    t.float  'profile_geo_long'
    t.float  'profile_geo_lat'
    t.string 'profile_geo_country_code'
    t.string 'profile_geo_region'
    t.string 'profile_geo_subregion'
    t.string 'profile_geo_locality'
    
    t.string 'profile_geo_name' #These are arrays, but currently will only have one item.

    t.datetime 'created_at'
    t.datetime 'updated_at'
end

create_table "actor_active", :force => true do |t|

    t.string  'native_id'
    t.integer 'activity_id'      #key into activities table
    t.integer 'followers_count'
    t.integer 'friends_count'
    t.integer 'statuses_count'
    t.integer 'klout_score'
    t.text    'topics'   #klout topics   #flattened array

    t.datetime 'created_at'
    t.datetime 'updated_at'
  end
```

###Storing Dynamic Metadata at Tweet Level with Select Metadata Array Tables


```
ActiveRecord::Schema.define(:version => 20140703221601) do

  create_table "activities", :force => true do |t|
    t.integer  "activity_id",     :limit => 8
    t.datetime "posted_at"
    t.text     "body"
    t.string   "verb"
    t.integer  "repost_of_id",    :limit => 8
    t.integer  "actor_id",        :limit => 8
    t.string   "lang"
    t.string   "generator"
    t.string   "link"
    t.string   "mentions"
    t.text     "urls"
    t.text     "media"
    t.string   "place"
    t.string   "country_code"
    t.float    "long"
    t.float    "lat"
    t.float    "long_box"
    t.float    "lat_box"
    t.integer  "followers_count"
    t.integer  "friends_count"
    t.integer  "statuses_count"
    t.integer  "klout_score"
    t.text     "payload"
    t.datetime "created_at",                   :null => false
    t.datetime "updated_at",                   :null => false
  end

  create_table "actors", :force => true do |t|
    t.integer  "actor_id",                 :limit => 8
    t.string   "handle"
    t.string   "display_name"
    t.string   "actor_link"
    t.string   "bio"
    t.string   "lang"
    t.string   "time_zone"
    t.integer  "utc_offset"
    t.datetime "posted_at"
    t.string   "location"
    t.string   "profile_geo_name"
    t.float    "profile_geo_long"
    t.float    "profile_geo_lat"
    t.string   "profile_geo_country_code"
    t.string   "profile_geo_region"
    t.string   "profile_geo_subregion"
    t.string   "profile_geo_locality"
    t.datetime "created_at",                            :null => false
    t.datetime "updated_at",                            :null => false
  end

  create_table "hashtags", :force => true do |t|
    t.integer  "activity_id", :limit => 8
    t.string   "hashtag"
    t.datetime "created_at",               :null => false
    t.datetime "updated_at",               :null => false
  end

  create_table "rules", :force => true do |t|
    t.integer  "activity_id", :limit => 8
    t.text     "value"
    t.string   "tag"
    t.datetime "created_at",               :null => false
    t.datetime "updated_at",               :null => false
  end

end
```










