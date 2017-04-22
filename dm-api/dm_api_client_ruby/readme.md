## Welcome Messages
Welcome to the @FloodSocial notification system! 
This websocket *Enroller* component is used for enrolling subscribers into a Twitter-based notification system. 
Users are asked to privately share their area of notification interest (along with their email if they 
want to receive email notifications). 
Fundamental Notification Components
* Enroller (this)
* Listener [code here](https://github.com/jimmoffitt/pi-adventures/blob/master/listen/stream/python/py-stream-pi.py)
* Notifier [code here](https://github.com/jimmoffitt/pi-adventures/blob/master/notify/tweet/post_tweet.rb)
This component uses the Twitter Direct Message API to  
This Twitter DM API client receives events from Twitter, and also sends Direct Messages.

## Notification metadata
Shared via DMs
* @handle
* placeOfInterest
* pointOfInterest
* email (could be configured elsewhere, metadata collected elsewhere)




# Notes

## PowerTrack fitlers
+ value: @source point_radius[long lat 25mi]
+ tag: @username-userID


@usgs_texasflood point_radius[97.7431 30.2672 25mi] 
@snowman

52 characters --> ~35 area-of-interests per rule

*One PowerTrack rule per user.* One real-time PowerTrack stream supports 250,000 rules, so one stream could scale to 250,000 users. If usihg the Search APIs to listen, you could poll for 30 users every minute using a single-threaded app making one request every two seconds. The Search API rate-limit defaults to 120 requests per minute, so ~4,200 users could be polled every minute.

Austin 30.2672° N, 97.7431° W  -->  97.7431 30.2672









# Outgoing message JSON

```json
{
  "direct_message_events": [
    {
      "type": "message_create",
      "id": "1234858585",
      "created_timestamp": "1392078023507",
      "message_create": {
        "target": {
          "recipient_id": "2244994945"
        },
        "sender_id": "3805104374",
        "message_data": {
          "text": "What's your favorite type of bird?",
          "entities": {
            "hashtags": [],
            "symbols": [],
            "urls": [],
            "user_mentions": [],
          },
          "quick_reply": {
            "type": "options",
            "options": [
              { "label": "Red Bird" , "metadata": "external_id_1"  },
              { "label": "Blue Bird" , "metadata": "external_id_2" },
              { "label": "Black Bird" , "metadata": "external_id_3" },
              { "label": "White Bird" , "metadata": "external_id_4" }
            ]
          },
          "attachment": {
            "type": "media",
            "media": {
             ...
            }
          }
        }
      }
    }
    ...
  ]
}

```

# Incoming message JSON

```json
{
  "direct_message_events": [
    {
      "type": "message_create",
      "id": "1234858592",
      "created_timestamp": "1392078023603",
      "message_create": {
        "target": {
          "recipient_id": "1234858592"
        },
        "sender_id": "3805104374",
        "message_data": {
          "text": "Blue Bird",
          "entities": {
            "hashtags": [],
            "symbols": [],
            "urls": [],
            "user_mentions": [],
          },
          "quick_reply_response": {
            "type": "options",
            "metadata": "external_id_2"
          },
          "attachment": {
            "type": "media",
            "media": {
             ...
            }
          }
        }
      }
    }
    ...
  ],
  "users": {
    "1234858592": {
      "id": "1234858592",
      "created_timestamp": "1415320482361",
      "name": "TwitterDev",
      "screen_name": "TwitterDev",
      "location": "Internet",
      "description": "Developer and Platform Relations @Twitter. We are developer advocates. We can't answer all your questions, but we listen to all of them!",
      "protected": false,
      "verified": true,
      "followers_count": 440643,
      "friends_count": 1534,
      "statuses_count": 2837,
      "profile_image_url": "http://pbs.twimg.com/profile_images/530814764687949824/npQQVkq8_normal.png",
      "profile_image_url_https": "https://pbs.twimg.com/profile_images/530814764687949824/npQQVkq8_normal.png"
    },
    "3805104374": {
      "id": "3805104374",
      "created_timestamp": "1449607341142",
      "name": "Furni",
      "screen_name": "furni",
      "location": "San Francisco, CA",
      "description": "Furni is Twitter's example company to showcase new developer features.",
      "protected": false,
      "verified": false,
      "followers_count": 297,
      "friends_count": 7,
      "statuses_count": 1,
      "profile_image_url": "http://pbs.twimg.com/profile_images/653712801219805185/S4LvoO9b_normal.png",
      "profile_image_url_https": "https://pbs.twimg.com/profile_images/653712801219805185/S4LvoO9b_normal.png"
    }
  }
}
```
