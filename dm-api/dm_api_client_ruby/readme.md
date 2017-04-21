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



