
# Enrolling subscribers to a notification service with the Twitter DM API

## Notificaton service details

Made of three components:
+ Enroller
+ Listener
+ Notifier

## Getting Started with the DM API


## Websocket client

Twitter DMs --> (private) user metadata --> client config

## Web app 

### Ruby and Sinatra

```ruby
require 'sinatra'

class EnrollerApp < Sinatra::Base
  get '/' do
    "Welcome to the @FloodSocial notification system!"
  end
  
   #Receives DM events
  post '/dm_event' do
      puts request.body
  end
end

```
### Node and Express

```node
var express = require('express');

/**
 * Receives DM events
 **/
app.post('/webhooks/twitter', function(request, response) {
  console.log(request.body.direct_message_events)
  response.send('200 OK');
});

app.listen(app.get('port'), function() {
  console.log('Node app is running on port', app.get('port'));
});

```

### Python and Flask
> any volunteers? One other system component, the Listener, is written in python 2.7.
