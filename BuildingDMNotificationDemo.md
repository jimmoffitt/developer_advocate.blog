# Enrolling subscribers to a geo-based notification service with the Twitter Direct Message API

+ [Introduction](#intro)
+ [Getting Started](#getting_started)
+ [Demo Design](#demo_design)
+ [Sample Code](#sample_code)

## Introduction <a id="intro" class="tall">&nbsp;</a>

In this tutorial we are going to build a geo-enabled notification system based on Twitter Direct Messages. The fundamental user-story is 

  _"As a Twitter user I want to receive notifications when a Twitter account of interest posts a geo-tagged Tweet that is in an area of interest."_ 

Notifications will come in the form of Twitter Direct Messages, the Twitter account of interest will post Tweets associated with an exact location, and areas of interest will be defined as a 25-mile circle centered on a location shared by the Twitter user. Users will be given two methods to *privately share* their location: select a point on a map, or chose from a list of locations. For this tutorial the location lists will consist of city names, and the corresponding areas will be based on the geographic center of those cities.

The fundamental goal of this tutorial is to demonstrate how to enable Twitter users to share their location of interest via a private Direct Message. The component we are building here is based on the Twitter Direct Message (DM) and Account Activity (AA) APIs. We will be referring to an *Enroller*, a webhook-based component that enables Twitter users to subscribe to the notification system. This *Enroller* is one of three components that make up the complete notification system:

  + Enroller - Webhook consumer that manages Direct Message events, both receiving messages from Twitter and also sending messages.
  + Alarm listener: monitors real-time stream for Tweets of (geo) interest, triggers notification when there is a location-of-interest match.
  + Notifier: send Direct Message notifications to subscribers. 
  
See [HERE] if you are interested in the other two components.
  
We'll start off with a discussion on how to get started with the Twitter Direct Message and Account Activity APIs. While the Direct Message API provides the communication method, the webhook-based Account Activity API enables real-time conversations between Twitter accounts. 
      
## Getting started with the Direct Message and Account Activity APIs <a id="getting_started" class="tall">&nbsp;</a>

A great place to start is our API documentation:

+ Twitter Webhook APIs https://dev.twitter.com/webhooks
+ Account Activity API documentation https://dev.twitter.com/webhooks/account-activity
+ Direct Message API methods: https://dev.twitter.com/rest/direct-messages

To help you put all that information together, check out our [Twitter Direct Message API Playbook].

As a developer, here are the steps you'll take:

Configuring Twitter Webhooks
+ Create a Twitter Application, and have it enabled with the Account Activity API 
+ Subscribe your consumer web app using the Account Activity API
+ Create a default Welcome Message 

Developing Webhook Consumer
+ Stand-up a web application that will be your webhook consumer 
+ Implement a consumer Challenge Response Check (CRC) method
+ Implement a consumer webhook event manager

Before we dig more into those steps, we'll describe the demo design that drove the development of this tutorial.

## Enroller Design <a id="design" class="tall">&nbsp;</a>

For this tutorial, a more specific user-story goes like:

   _"I would like to receive a Twitter Direct Message if there are official flood-related Tweets within my area of internet"_

A notification system that enables subscribers to privately share their location of interest, and receive private Direct Message notifications When geo-tagged Tweets with select attributes are posted within area(s) of interest.

This particular demo will use an existing Twitter-based data exchange system deployed by the USGS Texas Water Science Center (TXWSC). In 2016 two autonomous Twitter accounts were launched: @USGS_TexasFlood and @USGS_TexasRain. These accounts broadcast geo-tagged Tweets when any of ~750 sites around Texas record potential and current flood conditions. You can read more about this system at https://blog.twitter.com/2016/using-twitter-as-a-go-to-communication-channel-during-severe-weather-events. Although this demo is using the USGS Texas-based system, the techniques and APIs demonstrated here are agnostic beyond working with geo-tagged Tweets. This particular demo is based on geographical information, but other listening and user interest criteria is equally applicable. 

The main purposes of the Enroller are:

+ Introduce the user to the system.
+ Provide two ways to share locations of interest: 
  + Using a map.
  + Picking from a list of locations.
+ Enable subscribers to add multiple locations.   
+ Enable subscribers to review their current areas of interest.
+ Enable subscribers to unsubscribe.

From a developer's prespective, other desirable features included:
+ Ability to import a list of locations to display to user.
+ Provide Direct Message commands to use system.
    + DM 'Add' to add a new area of interest.
    + DM 'List' to review current areas of interest.
    + DM 'About' to learn more about system.
+ Abstract subscription details (user id and locations) into PowerTrack Rules.
    + This design was picked to keep the enroller light-weight by not implementing native data persistence. All user enrollments are stored remotely as a real-time PowerTrack ruleset. 



## Sample Code <a id="sample_code" class="tall">&nbsp;</a>

### Enroller helper objects
  + Scripts for configuration webhooks
  + Direct Message Event Manager 
  + Direct Message Content Manager
  + Direct Message Sender
  + Rule manager: takes user submissions and converts those to PowerTrack rules and adds to stream.

### Scripts for configuration webhooks

    + Using AA API, Assign webhook URL, generate webhook ID. 
    + Using DM API, create Default Welcome Message
    + Using AA API, add subscriber (webhook ID).

#### CRC Method 

As described [HERE](https://dev.twitter.com/webhooks/securing), Twitter will issue periodic Challenge Response Checks (CRC) and confirm the client app possesses the correct app secret. 


 + Implement GET method to support CRC.

``` ruby
require 'base64'

def generate_crc_response(consumer_secret, crc_token)
  hash = OpenSSL::HMAC.digest('sha256', consumer_secret, crc_token)
  return Base64.encode64(hash).strip!
end

# Receives challenge response check (CRC).
get '/webhooks/twitter' do
  crc_token = params['crc_token']
  response = {}
  response['response_token'] = "sha256=#{generate_crc_response(settings.dm_api_consumer_secret, crc_token)}"
  status 200
end

```

If not, webhook events will cease. While you are developing your webhook consumer you will need a tool that initiates a CRC check. During development it is likely that your web app will not be running during a Twitter challenge, and you'll need to trigger a challenge so you can re-establish webhook connectivity.

### Direct Message Event Manager 

```ruby

def handle_event(events)
  events = JSON.parse(events)
  if events.key? ('direct_message_events')
	  dm_events = events['direct_message_events']
		dm_events.each do | dm_event |
		  if dm_event['type'] == 'message_create'
 				#Is this a response? Test for the 'quick_reply_response' key.
				is_response = dm_event['message_create'] && dm_event['message_create']['message_data'] && dm_event['message_create']['message_data']['quick_reply_response']
				
				if is_response 
					response = dm_event['message_create']['message_data']['quick_reply_response']['metadata']
					user_id = dm_event['message_create']['sender_id']	  
					puts "User #{user_id} answered with #{response}"
          #Handle event
				else
				  #Since this DM is not a response to a QR, let's check for other 'action' commands
				  request = dm_event['message_create']['message_data']['text']
				  user_id = dm_event['message_create']['sender_id']
          #Handle request/command
 			  end  
     end   
  end
end
```



#### Webhook Event Receiver

### Direct Message Content Manager

### Direct Message Sender

### Rule Manager






================================================
Demo Notes:

+ Webhook consumer - Subscription Manager, private user data
+ Real-time PowerTrack with Rules API
  + Real-time PT stream consumer   
  + PowerTrack Rules Manager
+ DM Notifier
  


  
 + Develop Webhook (event) consumer.
  + Implement GET method to support CRC.
  + Implement POST method to receive webhook events from Twitter.
  + Build webhook event processor, forking conversations and building Direct Message content (welcome messages and quick replies).
  
  Fundamental pieces:
    + Configuration tools.
    + Event Manager - listening for responses and commands.
    + DM Sender - Asking questions and answering questions. 
    

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

This system is being deployed on the Twitter platform, which is a unique social media network since it has an emphasis on public communications. At the same time, Twitter offers private communcation with Direct Messages. These private messages enable subscribers to privately share their areas of interest. A key phrase here is "of interest." This notification system does not know the user's *current* and *precise* location, but rather it knows a more general area of interest. These areas of interest are (currently) circles with a 25-mile radius. 

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






