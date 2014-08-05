

####MySQL creation script
```
DROP TABLE IF EXISTS activities;
CREATE TABLE `activities` (
      `activity_id` BIGINT UNSIGNED NOT NULL DEFAULT 0                  
    , `posted_at` DATETIME NOT NULL DEFAULT '0000-00-00 00:00:00'       # activity postedTime
    , `payload` TEXT DEFAULT NULL                                       #Entire payload of JSON activity...?   
    , `body` TEXT DEFAULT NULL                                          # activity body text (whitespace padded) 
    , `verb` CHAR(16) DEFAULT NULL                                      # post/share/compliance 
    , `repost_of` BIGINT UNSIGNED NOT NULL DEFAULT 0                    #retweet 
    , `twitter_lang` CHAR(8) DEFAULT NULL                               # tweet body lang (tw) 
    , `gnip_lang` CHAR(8) DEFAULT NULL                                  # tweet body lang (gnip enrichment)
    , `generator` CHAR(255) DEFAULT NULL                                # service / platform
    , `link` CHAR(255) DEFAULT NULL                                     # www activity location url 

    #These are flattened arrays, comma delimited (?)
    , `hashtags` CHAR(140) DEFAULT NULL                                 # symbol (e.g. cashtag) 
    , `mentions` CHAR(140) DEFAULT NULL                                 # mention (display name) 
    , `urls` TEXT DEFAULT NULL                                          # url (t.co)      # media (photo) id
    , `media` TEXT DEFAULT NULL                                         # media (photo) expanded url
    , `rule_values` TEXT NOT NULL DEFAULT ''                            # Rule/filter that returned this data
    , `rule_tags` TEXT NOT NULL DEFAULT ''                              # Rules' associated tags

    #Activity geo details - Geo-tagged tweets only.
    , `place` CHAR(32) DEFAULT NULL
    , `country_code` CHAR(2) DEFAULT NULL
    , `long` DECIMAL(11,8) DEFAULT NULL                                 #Point.
    , `lat` DECIMAL(11,8) DEFAULT NULL                                  #Point.
    , `long_box` DECIMAL(11,8) DEFAULT NULL                             #If storing place bounding box.
    , `lat_box` DECIMAL(11,8) DEFAULT NULL                              #If storing place bounding box.

    #Actor metadata.
    , `actor_id` INT(16) UNSIGNED NOT NULL DEFAULT 0                    # numerical id  
    , `preferredUsername` CHAR(64) DEFAULT NULL 
    , `displayName` CHAR(64) DEFAULT NULL 
    , `actor_link` CHAR(255) DEFAULT NULL       
    , `bio` TEXT DEFAULT NULL  
    , `followers_count` INT(16) NOT NULL DEFAULT 0 
    , `friends_count` INT(16) NOT NULL DEFAULT 0 
    , `statuses_count` INT(16) NOT NULL DEFAULT 0 
    , `klout_score` INT(3) DEFAULT 0   
    , `klout_topics` TEXT DEFAULT NULL                                 #klout topics #flattened array.
    , `actor_lang` CHAR(8) DEFAULT NULL  
    , `time_zone` CHAR(255) DEFAULT NULL  
    , `utc_offset` INT(16) DEFAULT NULL  
    , `actor_created` DATETIME NOT NULL DEFAULT '0000-00-00 00:00:00'  #Actor.postedTime.    

    #Actor geo metadata.
    , `actor_location` CHAR(64) DEFAULT NULL                           #Twitter Profile location.
    #Only needed if Gnip Profile Geo enabled.
    #These really are flattened arrays, but currently will only have one item.
    , `profile_geo_name` CHAR(128) DEFAULT NULL
    , `profile_geo_long` DECIMAL(11,8) DEFAULT NULL                              
    , `profile_geo_lat` DECIMAL(11,8) DEFAULT NULL                                  , `profile_geo_country_code`
    , `profile_geo_region` CHAR(32) DEFAULT NULL
    , `profile_geo_subregion` CHAR(32) DEFAULT NULL 
    , `profile_geo_locality` CHAR(32) DEFAULT NULL 

    , `created_at` DATETIME NOT NULL DEFAULT '0000-00-00 00:00:00'  
    , `updated_at` DATETIME NOT NULL DEFAULT '0000-00-00 00:00:00'  
   
    #No duplicate activity ids!
    , PRIMARY KEY (`activity_id`)

    #Other keys?
    #, KEY (`act_table_id`)
 
    #Build your INDEXs here:
    # , FULLTEXT INDEX `body_idx` (`body`)  
    # , INDEX `postedTime_idx` (`postedTime`)
) 
ENGINE=MyISAM AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci
;

```

