
-------------------------------------------------

##Flood Project schema 

Tables: activities, hashtags, actors, rules

Activity table with dynamic actor attributes.

+ Activities table persists Activity obejects.
+ Storing 'dynamic' actor attributes on a activity-by-activity basis.
+ Metadata arrays are handled in two ways:
+     Separate metadata table with a many-to-one activity relationship.
+     Stored as a 'flattened' array with a specified delimiter.

Version summary:
hashtags and rules have own table.
mentions, URLs, and media do not.

Based on the assumption that this project dataset will consist of less that 5,000,000 tweets there will be a field for holding the entire JSON payload (which can average in 3KB uncompressed per tweet (retweets are bigger).

```
Activity model

 has many hashtags
 has many rules
 has one actor

```


Creation migration:

```
ActiveRecord::Schema.define(:version => 20140701000000) do

  create_table "activities", :force => true do |t|
    #t.integer 'id'               #ActiveRecord auto-incrementer
    t.integer 'activity_id'       #Business-logic Primary Key - no duplicate tweets.
    t.integer 'actor_id'          #User ID
    t.datetime 'posted_at'        #Always UTC.
    #t.text 'payload'             #Entire content of JSON activity...? 
    t.string 'body'               #140 (or more) characters. Tweet body, blog post?
    t.string 'verb'               #post,share, etc.
    t.integer 'repost_of_id'      #Points to original tweet.
    t.string 'lang'               #twitter_lang
    t.string 'generator'          #What platform did activity come from?
    t.string 'link'               #[Link to the activity](https://twitter.com/snowman/status/480209697199243264)

    #PowerTrack rules and associated tags are stored in a separate table.
    #'rules'                      #--> stored in separate hashtags table by activity ID.
    
    #These are twitter entities:
    #'hashtags'                   #--> stored in separate hashtags table by activity ID.
    #Flattened arrays, comma delimited for this project.
    t.string 'mentions'           # @mentions
    t.text 'urls'                 #Expanded URLs when available.
    t.string 'media'              #photos, vines, etc.
                             
    #Activity geo details - Geo-tagged tweets only.
    t.string 'place'
    t.string 'country_code'
    t.float 'long'
    t.float 'lat'
    t.float 'long_box' #If storing place bounding box.
    t.float 'lat_box'  

    #Dynamic 'actor' details.
    t.integer 'followers_count'
    t.integer 'friends_count'
    t.integer 'statuses_count'
    t.integer 'klout_score'
 
    #Standard ActiveRecord row metadata.
    #t.datetime 'created_at'
    #t.datetime 'updated_at'
  end
```

###Hashtags
```
ActiveRecord::Schema.define(:version => 20140624212018) do

  create_table "hashtags", :force => true do |t|

    #t.integer 'id'               #ActiveRecord auto-incrementer.
    t.integer 'activity_id'  
    t.string 'hashtag'

    #t.datetime 'created_at'
    #t.datetime 'updated_at'
```

###Rules (and tags)
```
ActiveRecord::Schema.define(:version => 20140624212018) do

  create_table "rules", :force => true do |t|

    #t.integer 'id'               #ActiveRecord auto-incrementer.
    t.integer 'activity_id'       #Duplicates expected.
    t.string 'value'
    t.string 'tag'
    
    #Standard ActiveRecord row metadata.
    #t.datetime 'created_at'
    #t.datetime 'updated_at'
```

###Actor metadata
```
ActiveRecord::Schema.define(:version => 20140624212018) do

  create_table actor", :force => true do |t|

    t.integer 'id'
    t.integer 'actor_id'
    t.string 'handle'
    t.string 'display_name'
    t.string 'link'
    t.string 'bio'
   
    t.string 'lang'
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
    
    #Standard ActiveRecord row metadata.
    #t.datetime 'created_at'
    #t.datetime 'updated_at'
```


###Rails Generator statements


####Activity table

activity activity_id:integer posted_at:datetime body:string verb:string repost_of_id:integer actor_id:integer lang:string generator:string link:string mentions:string urls:text media:text place:string country_code:string long:float lat:float long_box:float lat_box:float followers_count:integer friends_count:integer statuses_count:integer klout_source:integer payload:text

####Actor table

actor actor_id:integer handle:string display_name:string actor_link:string bio:string lang:string time_zone:string utc_offset:integer posted_at:datetime location:string profile_geo_name:string profile_geo_long:float profile_geo_lat:float profile_country_code:string profile_geo_region:string profile_geo_subregion:string profile_geo_locality:string

####Hashtag table

hashtag activity_id:integer hashtag:string

####Rules table

rule activity_id:integer value:string tag:string







