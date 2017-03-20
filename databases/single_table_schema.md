#### ActiveRecord Notes

* With Ruby-on-Rails' ActiveRecord there is a convention of every table having an "id" (auto-increment) field that is not explicitly shown in the 'create_table' method. Another convention is 'created_at' and 'updated_at' attributes are automatically updated by ActiveRecord when rows are added and updated.  
* Another convention is that if there is a _id field (like actor_id) that references the singular name of another table it is a foreign key into that separate table. Since this is single table example, this convention doesn't really come into play here. However, with schemas with an Actors table, an actor_id would by default/convention would be a foreign key into the Actors table.


```
ActiveRecord::Schema.define(:version => 20140624212018) do

  create_table "activities", :force => true do |t|
    t.integer 'activity_id'
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
    t.integer 'actor_id'
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
