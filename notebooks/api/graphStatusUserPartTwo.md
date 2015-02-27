---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/portal/pages/6774/preview
apiNotebookVersion: 1.1.66
title: Graph. Status, user part 2
---

```javascript

load('https://github.com/chaijs/chai/releases/download/1.9.0/chai.js')

```



See http://chaijs.com/guide/styles/ for assertion styles



```javascript

assert = chai.assert

```

```javascript

CLIENT_ID = prompt("Please, enter 'APP ID' of your facebook application.")

CLIENT_SECRET = prompt("Please, enter 'APP SECRET' of your facebook application.")

```

```javascript

ACHIEVEMENT_URL = prompt("Please, enter URL which points to a valid achievement definition.")

```

```javascript

// Read about the Facebook RAML API at https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/contracts

API.createClient('client', '/apiplatform/repository/public/organizations/30/apis/7965/versions/8129/definition');

```

```javascript

API.authenticate(client,"oauth_2_0",{

  clientId : CLIENT_ID,

  clientSecret : CLIENT_SECRET,

  scopes: [

    "manage_pages", "user_games_activity", "user_activities", "user_photos ", "user_likes", "user_events", "friends_events", "user_relationships", "read_stream", "manage_friendlists", "user_friends", "friends_relationships", "read_requests", "publish_actions"

  ]

})

```

```javascript

ACCESS_TOKEN = $5.accessToken//access the 'accessToken' field of the response of the API.authenticate() cell

```



Retrieve the Facebook pages of which the current user is an admin.



```javascript

accountsResponse = client.user_id("me").accounts.get()

```

```javascript

assert.equal( accountsResponse.status, 200 )

```



The Facebook apps that this person is a developer of.

Reading this endpoint returns an array of Application objects with the same fields as that node.



```javascript

applicationsDeveloperResponse = client.user_id("me").applications.developer.get()

```

```javascript

assert.equal( applicationsDeveloperResponse.status, 200 )

```



Reading this endpoint returns an array of Page objects (which represent each of the books)



```javascript

booksResponse = client.user_id("me").books.get()

```

```javascript

assert.equal( booksResponse.status, 200 )

```



The events this person is attending.



```javascript

//checkins API is deprecated for versions v2.0 and higher

checkinsResponse = client.user_id("me").checkins.get()

```

```javascript

assert.equal( checkinsResponse.status, 400 )

```



An array of Event objects.



```javascript

eventsResponse = client.user_id("me").events.get()

```

```javascript

assert.equal( eventsResponse.status, 200 )

```



Returns the events that the person is attending.



```javascript

attendingResponse = client.user_id("me").events.attending.get()

```

```javascript

assert.equal( attendingResponse.status, 200 )

```



Returns the events that the person has RSVP'd 'maybe'.



```javascript

maybeResponse = client.user_id("me").events.maybe.get()

```

```javascript

assert.equal( maybeResponse.status, 200 )

```



Returns the events that the person has declined to attend.



```javascript

declinedResponse = client.user_id("me").events.declined.get()

```

```javascript

assert.equal( declinedResponse.status, 200 )

```



Returns the events that the person has created.



```javascript

createdResponse = client.user_id("me").events.created.get()

```

```javascript

assert.equal( createdResponse.status, 200 )

```



Returns the events that the person has not yet RSVP'd.



```javascript

notRepliedResponse = client.user_id("me").events.not_replied.get()

```

```javascript

assert.equal( notRepliedResponse.status, 200 )

```



This person's family relationships.



```javascript

familyResponse = client.user_id("me").family.get()

```

```javascript

assert.equal( familyResponse.status, 200 )

```



The feed of posts (including status updates) and links published by this person, or by others on this person's profile.



```javascript

feedResponse = client.user_id("me").feed.get()

```

```javascript

assert.equal( feedResponse.status, 200 )

```



Publish a message.



```javascript

publishFeedResponse = client.user_id("me").feed.post({

  "message" : "This message was created by the 'facebook' API Notebook"

}, {headers:{"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( feedResponse.status, 200 )

statusId = publishFeedResponse.body.id

```



Retrieve status message



```javascript

statusResponse = client.status_id(statusId).get()

```

```javascript

assert.equal(statusResponse.status,200)

```



Delete a status message



```javascript

statusDeleteResponse = client.status_id(statusId ).delete()

```

```javascript

assert.equal(statusDeleteResponse.status,200)

```



A person's 'friend lists' - these are groupings of friends such as "Acquaintances" or "Close Friends", or any others that may have been created. They do not refer to the list of friends that a person has, which is accessed instead through the /{user-id}/friends edge.



```javascript

friendlistsResponse = client.user_id("me").friendlists.get()

```

```javascript

assert.equal( friendlistsResponse.status, 200 )

```



A person's pending friend requests.

This feature was removed after Graph API v1.0.



```javascript

//deprecated in version 2.0 and higher

//friendrequestsResponse = client.user_id("me").friendrequests.get()

```

```javascript

//assert.equal( friendrequestsResponse.status, 200 )

```



A person's friends.



```javascript

friendsResponse = client.user_id("me").friends.get()

```

```javascript

assert.equal( friendsResponse.status, 200 )

```



You can append another person's id or username to the edge in order to determine whether that person is friends with the root node user

If user-a is friends with user-b in the above request, the response will contain the User object for user-b. If they are not friends, there will be any empty dataset returned.



```javascript

anotherUserId = client.user_id("me").get().body.id

existingFriendResponse = client.user_id("me").friends.user_b_id(anotherUserId).get()

```

```javascript

assert.equal( existingFriendResponse.status, 200 )

```



The list of games someone has added to the Games > Likes section of their Facebook profile.



```javascript

gamesResponse = client.user_id("me").games.get()

```

```javascript

assert.equal( gamesResponse.status, 200 )

```