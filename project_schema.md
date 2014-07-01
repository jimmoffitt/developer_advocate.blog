
-------------------------------------------------

##Flood Project schema 


Storying Activities


+ Activities table persists Activity obejects.
+ Storing 'dynamic' actor attributes on a activity-by-activity basis.
+ 
```
ActiveRecord::Schema.define(:version => 20140624212018) do

  create_table "activities", :force => true do |t|
    t.integer 'id'
    t.integer 'activity_id'   #Primary Key
    t.datetime 'posted_at'
    t.text 'payload'          #Entire content of JSON activity...? 
    t.string 'body'
    t.string 'verb'
    t.integer 'repost_of'
    t.string 'twitter_lang'
    t.string 'generator'
    t.string 'link'

    
    #'hash_tags' --> stored in separate hashtags table by activity ID.
    #These are flattened arrays, comma delimited (?)
    t.string 'mentions' 
    t.text 'urls'         #Expanded URLs when available.
    t.string 'media'
    
    #How do we want to do this?
    t.text 'rule_values'  #Gnip PowerTrack matching rules. 
    t.text 'rule_tags'        
                            
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
 
    t.datetime 'created_at'
    t.datetime 'updated_at'
  end
```

###Hashtags
```
ActiveRecord::Schema.define(:version => 20140624212018) do

  create_table "hashtags", :force => true do |t|

    t.integer 'id'
    t.integer 'activity_id' 
    t.string 'hashtag'

    t.datetime 'created_at'
    t.datetime 'updated_at'
```

###Rules (and tags)
```
ActiveRecord::Schema.define(:version => 20140624212018) do

  create_table "rules", :force => true do |t|

    t.integer 'id'
    t.integer 'activity_id' 
    t.string 'value'
    t.string 'tag'
    t.datetime 'created_at'
    t.datetime 'updated_at'
```

###Actor metadata
```
ActiveRecord::Schema.define(:version => 20140624212018) do

  create_table actor", :force => true do |t|

    t.integer 'id'
    t.integer 'actor_id'
    t.string 'preferredUsername'
    t.string 'displayName'
    t.string 'actor_link'
    t.string 'bio'
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

```


