## Storing Social Media Data in Relational Databases (Part 4)

### Some Example Schemas

Below are some example schemas that provide a starting place for specifying your database schema. We start with a single-table schema that represents the most simple option. Then we provide example tables for segregating Actor attributes into separate 'static' and 'active' tables. Finally, we present the schema selected for our example flood use-case, one that stores dynamic Actor attributes at the tweet level, stores more static Actor attributes in a separate Actor table, and has separate tablles for select metadata arrays such as hashtags and matching rules.


### Compliance Details

At Twitter, we believe that respecting the privacy and intent of social media users is critically important to the long-term health of social platforms and the social data ecosystem. Consumers of social media data have a responsibility to honor the privacy and actions of end-users in order to create an environment of trust and respect. To assist in these efforts, Twitter/Gnip have provided the [Compliance API](http://support.gnip.com/apis/compliance_api/). Using the Compliance API makes it possible to be notified when Compliance events occur, such as deleted tweets and protected accounts. See [HERE](http://support.gnip.com/apis/compliance_api/api_reference.html#ComplianceEvents) for a review of the types of Compliance Events that can occur, and recommended actions for when they happen. 

To support those actions, here are some recommendations:

* When tweets are deleted we recommed that they are deleted from your database. Deleted tweets must not be displayed, processed or otherwise used by your Service.

* When Twitter accounts are deleted, they can be restored/undeleted within 30 days. Once an account is deleted or protected, none of its information and activities should be displayed, processed or otherwise used by your Service.  Since deleted accounts have a 30 day period during which they can be restored/undeleted, the database schema should have supporting fields for restored accounts (see below for details). After 30 days a deleted account should then be dropped. Note that after the 30-day period, separate "deleted tweet" events for all received tweets will be served by the Compliance API.
 
* Suggested database schema features to support Compliance events:
  * Activity table:
    * unavailable (boolean) - a flag for deleted or withheld tweets. When set to 'true' these tweets must not be incorporated by your Service.    
  * Actor table: 
    * unavailable (boolean) - a flag for deleted or protected accounts. When set to 'true' these tweets must not be incorporated by your Service. 
    * unavailable_time (timestamp) - Used to track when a deleted account has passed the "restore" period and should be deleted.

_Note for "Geo Scrubbing" Compliance events:_ when a "geo_scrub" Compliance event occurs for an Actor/Account, tweets from that account should have all previous geo-tagged tweets "scrubbed" of their tweet geographical metadata. Specifically, metadata associated with the following objects should be nulled: 

* "coordinates" (Original format)
* "geo" (Original and Activity Stream formats)
* "location" (Activity Stream format)
* "place" (Original format) 

### Creating Databases

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
|    1000        | 377853980170141696 |    123   |     17200003       |    Tweeting in the Rain, Part 3 http://t.co/qCtH3ZucAT via @gnip |

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
AND act.handle = 'snowman'
AND act.unavailable = 0;     #Not a deleted or protected account.
```


### Single Activities Table

As discussed above, this design has the disadvantage of inefficiently storing redundant data, but it should be adequate for many use-cases such as datasets from Historical PowerTrack jobs with a finite amount of data.

The following example illustrates the most basic schema, where all metadata is stored at the activity level. This design has an disadvantage of being less efficient with respect to (mostly) static metadata. Also, metadata arrays, such as hashtags and mentions, are stored in a single field.

[Creating with Ruby ActiveRecord](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/single_table_schema.md)


```
ActiveRecord::Schema.define(:version => 20140624212018) do

  create_table 'activities', :force => true do |t|

    #t.integer 'id'           #auto-increment field that other tables' activity_id foreign keys refer to.      
    t.string 'tweet_id'
    t.boolean 'tweet_unavailable', :default => false
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
    t.boolean 'actor_unavailable', :default => false
    t.datetime 'actor_unavailable_at'
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
create_table 'actors_static', :force => true do |t|
    t.string 'user_id'
    t.boolean 'unavailable' :default => false
    t.datetime 'unavailable_at'
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

create_table 'actor_active', :force => true do |t|
    t.string  'user_id'
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

### Storing Dynamic Metadata at Tweet Level with Select Metadata Array Tables


```
ActiveRecord::Schema.define(:version => 20140703221601) do

  create_table 'activities', :force => true do |t|
    t.integer  'tweet_id',     :limit => 8
    t.boolean  'unavailable', :default => false
    t.datetime 'posted_at'
    t.text     'body'
    t.string   'verb'
    t.integer  'repost_of_id',    :limit => 8
    t.integer  'user_id',        :limit => 8
    t.string   'lang'
    t.string   'generator'
    t.string   'link'
    t.string   'mentions'
    t.text     'urls'
    t.text     'media'
    t.string   'place'
    t.string   'country_code'
    t.float    'long'
    t.float    'lat'
    t.float    'long_box'
    t.float    'lat_box'
    t.integer  'followers_count'
    t.integer  'friends_count'
    t.integer  'statuses_count'
    t.integer  'klout_score'
    t.text     'payload'
    t.datetime 'created_at',                   :null => false
    t.datetime 'updated_at',                   :null => false
  end

  create_table 'actors', :force => true do |t|
    t.integer  'user_id',                 :limit => 8
    t.boolean  'unavailable', :default => false
    t.datetime 'unavailable_at' 
    t.string   'handle'
    t.string   'display_name'
    t.string   'actor_link'
    t.string   'bio'
    t.string   'lang'
    t.string   'time_zone'
    t.integer  'utc_offset'
    t.datetime 'posted_at'
    t.string   'location'
    t.string   'profile_geo_name'
    t.float    'profile_geo_long'
    t.float    'profile_geo_lat'
    t.string   'profile_geo_country_code'
    t.string   'profile_geo_region'
    t.string   'profile_geo_subregion'
    t.string   'profile_geo_locality'
    t.datetime 'created_at',                            :null => false
    t.datetime 'updated_at',                            :null => false
  end

  create_table 'hashtags', :force => true do |t|
    t.integer  'tweet_id', :limit => 8
    t.string   'hashtag'
    t.datetime 'created_at',               :null => false
    t.datetime 'updated_at',               :null => false
  end

  create_table 'rules', :force => true do |t|
    t.integer  'tweet_id', :limit => 8
    t.text     'value'
    t.string   'tag'
    t.datetime 'created_at',               :null => false
    t.datetime 'updated_at',               :null => false
  end

end
```

### Schema used for flood project.


### Previous:
* [Introduction](https://github.com/twitterdev/dev-articles/blob/master/relational_db/db_intro.md)
* [Storing Metadata Arrays](https://github.com/twitterdev/dev-articles/blob/master/relational_db/db_metadata_arrays.md)
* [Storing Dynamic and Static Metadata](https://github.com/twitterdev/dev-articles/blob/master/relational_db/db_static_data.md)
