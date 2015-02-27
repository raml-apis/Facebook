---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/portal/pages/6779/preview
apiNotebookVersion: 1.1.66
title: Graph. heckins, domains, events
---

```javascript

load('https://github.com/chaijs/chai/releases/download/1.9.0/chai.js')

```



See http://chaijs.com/guide/styles/ for assertion styles



```javascript

var assert = chai.assert;

```

```javascript

CLIENT_ID = prompt("Please, enter 'APP ID' of your facebook application.")

CLIENT_SECRET = prompt("Please, enter 'APP SECRET' of your facebook application.")

```



A web site domain within the Graph API used by this notebook. To register your own Domain, you must claim your domain name using Facebook Insights.



```javascript

domainName = "https://www.facebook.com/positivchik.smith";

```

```javascript

// Read about the Facebook RAML API at https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/contracts

API.createClient('client', '/apiplatform/repository/public/organizations/30/apis/7965/versions/8129/definition');

```

```javascript

API.authenticate(client,"oauth_2_0",{

  clientId: CLIENT_ID,

  clientSecret: CLIENT_SECRET,

  scopes: [

    "manage_pages", "user_games_activity", "user_activities", "user_photos ", "user_likes", "user_events", "friends_events", "user_relationships", "read_stream", "manage_friendlists", "user_friends", "friends_relationships","rsvp_event", "user_groups",

    "read_requests", "publish_actions", "publish_stream"

  ]

});

```

```javascript

ACCESS_TOKEN = $5.accessToken//access the 'accessToken' field of the response of the 4th cell which is the result of API.authenticate()

```



Retrieving the application access token.



```javascript

appTokenResponse = client("/oauth/access_token").get({

  client_id : CLIENT_ID,

  client_secret : CLIENT_SECRET,

  grant_type : "client_credentials"

})

```

```javascript

APP_ACCESS_TOKEN = appTokenResponse.body.substring(appTokenResponse.body.indexOf("=")+1);

```



Retrieve own user_id



```javascript

OWN_USER_ID = client.user_id("me").get().body.id

```



Retrieve a domain.



```javascript

domainResponse = client.domain_id(domainName).get();

```

```javascript

assert.equal(domainResponse.status, 200);

dimainId = domainResponse.body.id

```



Facebook Insights is a product available to all Pages and Apps on Facebook, and any domains claimed by a site developer using the Insights dashboard. This object represents a single Insights metric that is tied to another particular Graph API object (Page, Post, etc.). 



```javascript

insightResponse = client.domain_id(dimainId).insights.get();

```

```javascript

assert.equal(insightResponse.status, 200);

```



Retrieve some event.



```javascript

events = client.user_id("me").events.created.get();

eventId = events.body.data[0].id;

```

```javascript

eventResponse = client.event_id(eventId).get();

```

```javascript

assert.equal(eventResponse.status, 200);

```



Get the feed of posts (including status updates) and links published to this event's wall.



```javascript

feedResponse = client.event_id(eventId).feed.get()

```

```javascript

assert.equal(feedResponse.status, 200);

```



This connection corresponds to the Event's wall. You can create a link, post or status message by issuing an HTTP POST request to the EVENT_ID/feed connection



```javascript

feedPostResponse = client.event_id(eventId).feed.post({

  "access_token" : ACCESS_TOKEN,

  "message": "Test message"

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```

```javascript

assert.equal(feedPostResponse.status, 200);

messageId = feedPostResponse.body.id

```



Delete the test message.



```javascript

client.object_id(messageId).delete()

```



You can read the list of invitees for an event by issuing an HTTP GET to /EVENT_ID/invite

User should already be invited



```javascript

invitedResponse = client.event_id(eventId).invited.get();

```

```javascript

assert.equal(invitedResponse.status, 200);

```



You can check whether a specific user has been invited to an event by issuing an HTTP GET to /EVENT_ID/invited/USER_I



```javascript

userInvitedResponse = client.event_id(eventId).invited.USER_ID(OWN_USER_ID).get();

```

```javascript

assert.equal(userInvitedResponse.status, 200);

```



You can un-invite a user from an event by issuing an HTTP DELETE to /EVENT_ID/invited/USER_ID. Returns true if the request is successful. The user must be an admin (i.e. creator) of the event for this call to succeed. Requires create_event permission



Does not work. "Requires extended permission: rsvp_event" received. But the rsvp_event is already in the scope.



```javascript

userEventDeleteResponse = client.event_id(eventId).invited.USER_ID(OWN_USER_ID).delete();

```

```javascript

assert.equal( userEventDeleteResponse.status, 200 );

```



You can see which users are 'attending' an event by issuing an HTTP GET to /EVENT_ID/attending This returns a list of all users who responded 'yes' to the even



Attend user should manually



```javascript

attendingResponse = client.event_id(eventId).attending.get();

```

```javascript

assert.equal(attendingResponse.status, 200);

```



You can RSVP the user as 'attending' an Event by issuing an HTTP POST to EVENT_ID/attending. This requires the rsvp_event permission and returns true if the RSVP is successful, and false otherwise



RSVP property already has. Method does not work.



```javascript

//attendingResponse = client.event_id(eventId).attending.post({

//	"access_token" : ACCESS_TOKEN

//}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```

```javascript

//assert.equal(attendingResponse.status, 200);

```



You can check if a specific user responded 'yes' to an event by issuing an HTTP GET to /EVENT_ID/attending/USER_ID. These operations require the user_events or friends_events permissions for non-public events and return an array of objects with the name, id, and rsvp_status fields. When checking a single user, an empty data will be returned if the user did not respond 'yes' to the event



```javascript

userAttendingResponse = client.event_id(eventId).attending.USER_ID(OWN_USER_ID).get();

```

```javascript

assert.equal(userAttendingResponse.status, 200);

```



You can see which users have replied 'maybe' to an event by issuing an HTTP GET to /EVENT_ID/maybe This returns a list of all users who responded 'maybe' to the even



```javascript

maybeResponse = client.event_id(eventId).maybe.get();

```

```javascript

assert.equal(maybeResponse.status, 200);

```



You can RSVP the user as a 'maybe' for an Event by issuing an HTTP POST to EVENT_ID/maybe. This requires the rsvp_event permission and returns true if the RSVP is successful, and false otherwise



RSVP property already has. Method does not work.



```javascript

//maybeResponse = client.event_id(eventId).maybe.post({

//	"access_token" : ACCESS_TOKEN

//}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```

```javascript

//assert.equal(maybeResponse.status, 200);

```



You can check if a specific user responded 'maybe' to an event by issuing an HTTP GET to /EVENT_ID/maybe/USER_ID. These operations require the user_events or friends_events permissions for non-public events and return an array of objects with the name, id, and rsvp_status fields. When checking a single user, an empty data will be returned if the user did not respond 'maybe' to the event



```javascript

userMaybeResponse = client.event_id(eventId).maybe.USER_ID(OWN_USER_ID).get();

```

```javascript

assert.equal(userMaybeResponse.status, 200);

```



You can see which users are declined an event (i.e. responded 'no') by issuing an HTTP GET to /EVENT_ID/declined This returns a list of all users who responded 'no' to the even



```javascript

declinedResponse = client.event_id(eventId).declined.get();

```

```javascript

assert.equal(declinedResponse.status, 200);

```



You can RSVP the user as 'declined' for an Event by issuing an HTTP POST to EVENT_ID/declined. This requires the rsvp_event permission and returns true if the RSVP is successful, and false otherwise



RSVP property already has. Method does not work.



```javascript

//declinedResponse = client.event_id(eventId).declined.post({

//	"access_token" : ACCESS_TOKEN

//}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```

```javascript

//assert.equal(declinedResponse.status, 200);

```



You can check if a specific user responded 'no' to an event by issuing an HTTP GET to /EVENT_ID/declined/USER_ID. These operations require the user_events or friends_events permissions for non-public events and return an array of objects with the name, id, and rsvp_status fields. When checking a single user, an empty data will be returned if the user did not respond 'no' to the event



```javascript

userDeclineResponse = client.event_id(eventId).declined.USER_ID(OWN_USER_ID).get();

```

```javascript

assert.equal(userDeclineResponse.status, 200);

```



You can see which users have not replied to an event by issuing an HTTP GET to /EVENT_ID/noreply This returns a list of all users who have not replied to the even



```javascript

noreplyResponse = client.event_id(eventId).noreply.get();

```

```javascript

assert.equal(noreplyResponse.status, 200);

```



You can check if a specific user has not replied to an event by issuing an HTTP GET to /EVENT_ID/noreply/USER_ID. These operations require the user_events or friends_events permissions for non-public events and return an array of objects with the name, id, and rsvp_status fields. When checking a single user, an empty data will be returned if the user did reply to the event



```javascript

userNoreplyResponse = client.event_id(eventId).noreply.USER_ID(OWN_USER_ID).get();

```

```javascript

assert.equal(userNoreplyResponse.status, 200);

```



You can read the photos published to an event by issuing an HTTP GET request to /EVENT_ID/photos with a user access_token



```javascript

photosResponse = client.event_id(eventId).photos.get();

```

```javascript

assert.equal(photosResponse.status, 200);

```



You can post photos to an Event's Wall by issuing an HTTP POST request to EVENT_ID/photos with the publish_stream permissio



```javascript

photosResponse = client.event_id(eventId).photos.post({

  "access_token" : ACCESS_TOKEN,

  "url":"https://pp.vk.me/c616521/v616521297/2033e/J0kgnDd5ie0.jpg"

},{headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```

```javascript

assert.equal(photosResponse.status, 200);

photoId = photosResponse.body.id

```



Remove the photo



```javascript

client.photo_id(photoId).delete()

```



You can read the videos published to an event by issuing an HTTP GET request to /EVENT_ID/videos with a user access_token



Event already should have uploaded videos.



```javascript

videosResponse = client.event_id(eventId).videos.get();

```

```javascript

assert.equal(videosResponse.status, 200);

```



An event's cover photo.



```javascript

pictureResponse = client.event_id(eventId).picture.get();

```

```javascript

assert.equal(pictureResponse.status, 200);

```



You can add a profile picture to an event by issuing an HTTP POST request to /EVENT_ID/picture with the create_event permission for an admin of the even



```javascript

// events management API is deprecated for versions v2.0 and higher

pictureResponse = client.event_id(eventId).picture.post({

  "access_token" : ACCESS_TOKEN,

  "url":"https://pp.vk.me/c616521/v616521297/2033e/J0kgnDd5ie0.jpg"

},{headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```

```javascript

assert.equal(pictureResponse.status, 400);

```



You can delete an event's profile picture by issuing an HTTP DELETE request to /EVENT_ID/picture with the create_event permission for an admin of the event



```javascript

//events management API is deprecated for versions v2.0 and higher

pictureResponse = client.event_id(eventId).picture.delete();

```

```javascript

assert.equal(pictureResponse.status, 400);

```



Retrieve a list of checkins.



```javascript

//checkins API is deprecated for versions v2.0 and higher

checkinsResponse = client.user_id("me").checkins.get();

```

```javascript

assert.equal(checkinsResponse.status,400)

checkinId = null//checkinsResponse.body.data[0].id;

```

```javascript

if(checkinId){

  commentsResponse = client.checkin_id(checkinId).comments.post({

    "message": "messageValue"

  });

}

```

```javascript

if(checkinId){

  assert.equal( commentsResponse.status, 200 );

}

```



You can like a checkin by issuing a HTTP POST request to CHECKIN_ID/likes connection with the publish_stream permission. No parameters are necessary



```javascript

if(checkinId){

  likesResponse = client.checkin_id(checkinId).likes.post();

}

```

```javascript

if(checkinId){

  assert.equal( likesResponse.status, 200 );

}

```



You can unlike an checkin by issuing an HTTP DELETE request to the CHECKIN_ID/likes connection with the publish_stream permission



```javascript

if(checkinId){

  likesResponse = client.checkin_id(checkinId).likes.delete();

}

```

```javascript

if(checkinId){

  assert.equal( likesResponse.status, 200 );

}

```