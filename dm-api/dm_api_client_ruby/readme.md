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
## PowerTrack fitlers
value: @source point_radius[long lat 25mi]
tag: @username-userID
