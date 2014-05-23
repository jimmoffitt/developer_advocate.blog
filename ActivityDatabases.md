##Storing Social Media Data in Relational Databases

###Introduction

Many consumers of social media data store it in a relational database such as MySQL. 

There are several key details to ponder as you design your database schema.

* One is deciding what information to store and what, if any, to ignore/drop.
* One is handling metadata that has a variable amount of members/items. Examples include hashtags, user mentions and URLs.
* Do you need to track updates to attributes such as a user's profile location and followers count?
* Do you need to store data from multiple publishers?
 

In this article we'll discuss some fundamental decisions that need to be made, various options when designing your database schema, and provide some example schemas for getting started.

###What Metadata do you need to store?

###Handing Metadata with Variable Number of Items

###Tracking Time-series Changes


###Some Example Schemas



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
