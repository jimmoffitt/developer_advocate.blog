-------------------------------------------------

## Flood Project schema 

Tables: activities, hashtags, actors, rules

Activity table with dynamic actor attributes.

+ Activities table persists Activity objects.
     + Storing 'dynamic' actor attributes on a activity-by-activity basis.
+ More static actor atributes are in Actor table.
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
ActiveRecord::Schema.define(:version => 20140703221601) do

  create_table "activities", :force => true do |t|
    t.integer  "tweet_id",     :limit => 8
    t.datetime "posted_at"
    t.text     "body"
    t.string   "verb"
    t.integer  "repost_of_id",    :limit => 8
    t.integer  "user_id",        :limit => 8
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
    t.integer  "user_id",                 :limit => 8
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
    t.integer  "tweet_id", :limit => 8
    t.string   "hashtag"
    t.datetime "created_at",               :null => false
    t.datetime "updated_at",               :null => false
  end

  create_table "rules", :force => true do |t|
    t.integer  "tweet_id", :limit => 8
    t.text     "value"
    t.string   "tag"
    t.datetime "created_at",               :null => false
    t.datetime "updated_at",               :null => false
  end

end

```

### Rails Generator statements

#### Activity table

activity tweet_id:integer posted_at:datetime body:string verb:string repost_of_id:integer user_id:integer lang:string generator:string link:string mentions:string urls:text media:text place:string country_code:string long:float lat:float long_box:float lat_box:float followers_count:integer friends_count:integer statuses_count:integer klout_source:integer payload:text

#### Actor table

actor user_id:integer handle:string display_name:string actor_link:string bio:string lang:string time_zone:string utc_offset:integer posted_at:datetime location:string profile_geo_name:string profile_geo_long:float profile_geo_lat:float profile_country_code:string profile_geo_region:string profile_geo_subregion:string profile_geo_locality:string

#### Hashtag table

hashtag tweet_id:integer hashtag:string

#### Rules table

rule tweet_id:integer value:string tag:string



