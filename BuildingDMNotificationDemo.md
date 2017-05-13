# Enrolling subscribers to a notification service with the Twitter DM API

# Introduction

  [] Twitter Webhook consumer
  [] Rule manager: takes user submissions and converts those to PowerTrack rules and adds to stream.
  [] Alarm listener: monitors real-time stream for Tweets of (geo) interest,
  [] Notifier: send DM Notifications to subscribers. 

Demo Design


Demo Details:

+ Webhook consumer - Subscription Manager, private user data
+ Real-time PowerTrack with Rules API
  + Real-time PT stream consumer   
  + PowerTrack Rules Manager
+ DM Notifier
  





# Details

# Set-up steps


+ Set-up Twitter details
  + Get access to Account Activity API (AA API)
  + Get access to Direct Message API
  + Set up Webhooks configuration
    + Using AA API, Assign webhook URL, generate webhook ID. 
    + Using DM API, create Default Welcome Message
    + Using AA API, add subscriber (webhook ID).
  
 + Develop Webhook (event) consumer.
  + Implement GET method to support CRC.
  + Implement POST method to receive webhook events from Twitter.
  + Build webhook event processor, forking conversations and building Direct Message content (welcome messages and quick replies).
    

[] Deploy Webhook consusumer.
    [] Developing with localhost
        [] ngrok and pagekite experiments.
           ./ngrok http -subdomain=customdomain 9393
    [] Deploying to cloud host
        [] heroku notes:
    [] Consumer data transfers to other components? Data stores?
        [] Maintain simple text file?
        [] Write to database?
        [X] Sneakers and thumbdrives? 

[] Other concerns outside of the webhook component:
  [] Rule manager: takes user submissions and converts those to PowerTrack rules and adds to stream.
  [] Alarm listener: monitors real-time stream for Tweets of (geo) interest,
  [] Notifier: send DM Notifications to subscribers. 


## Having users share their location.

In order to prototype a geo-aware notification system, knowing subscriber locations of interest is key metadata. 

This system is being deployed on the Twitter platform, which is a unique social media network since it has an emphasis on public communications. At the same time, Twitter has a fundamental and core value of defending and respecting the user's voice. This includes protecting details that user's want to keep private, such as their email addresses and private messages. It also includes only sharing their current locations when they want to. Subscribers to this system should not have to share their locations in a public matter. And with new platform udpates, it is now easy to enable users to share their location of interest using private Direct Messages.

A key word here is "of interest." This notification system does not know the user's *current* and *precise* location, but rather it knows a more general area of interest. These areas of interest are (currently) circles with a 25-mile radius. 

The prototye gives users two options for sharing their location of intertest:

+ Pick from a list of locations. For this prototype is a list of Texas cities.
+ Select location on a map. This map defaults to your device's current location, and is scrollable so you can select any location.

When a user pick from the list, 

answer = user_dm['direct_message_events']['message_create']['message_data']['quick_reply_response']
if answer == 'list'
if answer == 'map'
if answer == 'unsubscribe'

user_id = user_dm['direct_message_events']['message_create']['sender_id']


user_dm['direct_message_events']['message_create']['message_data']['entities']['urls']['expanded_url']
-->  "https://twitter.com/i/location?lat=32.71790004525148&long=-97.32473787097997"


user_dm['direct_message_events']['message_create']['message_data']['attachment']['location']['shared_coordinates']['coordinates']['coordinates'] --> array --> 
['coordinates'][0] --> -97.032########  
['coordinates'][1] --> 32.7179000##### 


## From subscriptions to real-time PowerTrack 





## Example JSON

### User picked 'share location' method:


```
"direct_message_events": [
    {
      "type": "message_create",
      "id": "862053336907894788",
      "created_timestamp": "1494364506352",
      "message_create": {
        "target": {
          "recipient_id": "17200003"
        },
        "sender_id": "944480690",
        "message_data": {
          "text": "Pick area of interest from list",
          "entities": {
            "hashtags": [
              
            ],
            "symbols": [
              
            ],
            "user_mentions": [
              
            ],
            "urls": [
              
            ]
          },
          "quick_reply_response": {
            "type": "options",
            "metadata": "location_list"
          }
        }
      }
    }
  ],
  "users": {
    "17200003": {
      "id": "17200003",
      "created_timestamp": "1225926397000",
      "name": "Think Snow \u2744\ufe0f\u2603\ud83d\udca7\ud83c\udf0e",
      "screen_name": "snowman",
      "location": "Longmont, Colorado",
      "description": "family, travel, music, snow, urban farming, photography, coding, weather, hydrology, early-warning systems. From Minnesota, live in Colorado,",
      "protected": false,
      "verified": false,
      "followers_count": 675,
      "friends_count": 408,
      "statuses_count": 1379,
      "profile_image_url": "http:\/\/pbs.twimg.com\/profile_images\/697445972402540546\/2CYK3PWX_normal.jpg",
      "profile_image_url_https": "https:\/\/pbs.twimg.com\/profile_images\/697445972402540546\/2CYK3PWX_normal.jpg"
    },
    "944480690": {
      "id": "944480690",
      "created_timestamp": "1352750395000",
      "name": "demo account",
      "screen_name": "FloodSocial",
      "location": "Burlington, MA USA",
      "description": "Testing, testing, 1, 2, 3, testing. Used for testing, but also for demoing the Twitter platform... Coming soon?",
      "url": "https:\/\/t.co\/iTHxRCia2w",
      "protected": false,
      "verified": false,
      "followers_count": 29,
      "friends_count": 10,
      "statuses_count": 68,
      "profile_image_url": "http:\/\/pbs.twimg.com\/profile_images\/2880307445\/a6de07ce4053d93d9dd8db5f56f210ec_normal.png",
      "profile_image_url_https": "https:\/\/pbs.twimg.com\/profile_images\/2880307445\/a6de07ce4053d93d9dd8db5f56f210ec_normal.png"
    }
  }
}
```


### User picked area of interest from location list:

```
{
  "direct_message_events": [
    {
      "type": "message_create",
      "id": "862160427215712260",
      "created_timestamp": "1494390038671",
      "message_create": {
        "target": {
          "recipient_id": "17200003"
        },
        "sender_id": "944480690",
        "message_data": {
          "text": "Austin",
          "entities": {
            "hashtags": [
              
            ],
            "symbols": [
              
            ],
            "user_mentions": [
              
            ],
            "urls": [
              
            ]
          },
          "quick_reply_response": {
            "type": "options"
          }
        }
      }
    }
  ],
  "users": {
    "17200003": {
      "id": "17200003",
      "created_timestamp": "1225926397000",
      "name": "Think Snow \u2744\ufe0f\u2603\ud83d\udca7\ud83c\udf0e",
      "screen_name": "snowman",
      "location": "Longmont, Colorado",
      "description": "family, travel, music, snow, urban farming, photography, coding, weather, hydrology, early-warning systems. From Minnesota, live in Colorado,",
      "protected": false,
      "verified": false,
      "followers_count": 675,
      "friends_count": 408,
      "statuses_count": 1380,
      "profile_image_url": "http:\/\/pbs.twimg.com\/profile_images\/697445972402540546\/2CYK3PWX_normal.jpg",
      "profile_image_url_https": "https:\/\/pbs.twimg.com\/profile_images\/697445972402540546\/2CYK3PWX_normal.jpg"
    },
    "944480690": {
      "id": "944480690",
      "created_timestamp": "1352750395000",
      "name": "demo account",
      "screen_name": "FloodSocial",
      "location": "Burlington, MA USA",
      "description": "Testing, testing, 1, 2, 3, testing. Used for testing, but also for demoing the Twitter platform... Coming soon?",
      "url": "https:\/\/t.co\/iTHxRCia2w",
      "protected": false,
      "verified": false,
      "followers_count": 29,
      "friends_count": 10,
      "statuses_count": 68,
      "profile_image_url": "http:\/\/pbs.twimg.com\/profile_images\/2880307445\/a6de07ce4053d93d9dd8db5f56f210ec_normal.png",
      "profile_image_url_https": "https:\/\/pbs.twimg.com\/profile_images\/2880307445\/a6de07ce4053d93d9dd8db5f56f210ec_normal.png"
    }
  }
}


```

### User picked area of interest from map:

```

{
  "direct_message_events": [
    {
      "type": "message_create",
      "id": "862154965623689219",
      "created_timestamp": "1494388736526",
      "message_create": {
        "target": {
          "recipient_id": "17200003"
        },
        "sender_id": "944480690",
        "message_data": {
          "text": " https:\/\/t.co\/JyjKciQmnt",
          "entities": {
            "hashtags": [
              
            ],
            "symbols": [
              
            ],
            "user_mentions": [
              
            ],
            "urls": [
              {
                "url": "https:\/\/t.co\/JyjKciQmnt",
                "expanded_url": "https:\/\/twitter.com\/i\/location?lat=32.71790004525148&long=-97.32473787097997",
                "display_url": "twitter.com\/i\/location?lat\u2026",
                "indices": [
                  1,
                  24
                ]
              }
            ]
          },
          "attachment": {
            "type": "location",
            "location": {
              "type": "shared_coordinate",
              "shared_coordinate": {
                "coordinates": {
                  "type": "Point",
                  "coordinates": [
                    -97.32473787098,
                    32.717900045251
                  ]
                }
              }
            }
          },
          "quick_reply_response": {
            "type": "location",
            "metadata": "not used, useful?"
          }
        }
      }
    }
  ],
  "users": {
    "17200003": {
      "id": "17200003",
      "created_timestamp": "1225926397000",
      "name": "Think Snow \u2744\ufe0f\u2603\ud83d\udca7\ud83c\udf0e",
      "screen_name": "snowman",
      "location": "Longmont, Colorado",
      "description": "family, travel, music, snow, urban farming, photography, coding, weather, hydrology, early-warning systems. From Minnesota, live in Colorado,",
      "protected": false,
      "verified": false,
      "followers_count": 675,
      "friends_count": 408,
      "statuses_count": 1380,
      "profile_image_url": "http:\/\/pbs.twimg.com\/profile_images\/697445972402540546\/2CYK3PWX_normal.jpg",
      "profile_image_url_https": "https:\/\/pbs.twimg.com\/profile_images\/697445972402540546\/2CYK3PWX_normal.jpg"
    },
    "944480690": {
      "id": "944480690",
      "created_timestamp": "1352750395000",
      "name": "demo account",
      "screen_name": "FloodSocial",
      "location": "Burlington, MA USA",
      "description": "Testing, testing, 1, 2, 3, testing. Used for testing, but also for demoing the Twitter platform... Coming soon?",
      "url": "https:\/\/t.co\/iTHxRCia2w",
      "protected": false,
      "verified": false,
      "followers_count": 29,
      "friends_count": 10,
      "statuses_count": 68,
      "profile_image_url": "http:\/\/pbs.twimg.com\/profile_images\/2880307445\/a6de07ce4053d93d9dd8db5f56f210ec_normal.png",
      "profile_image_url_https": "https:\/\/pbs.twimg.com\/profile_images\/2880307445\/a6de07ce4053d93d9dd8db5f56f210ec_normal.png"
    }
  }
}

```






