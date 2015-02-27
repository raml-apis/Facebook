---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/portal/pages/6773/preview
apiNotebookVersion: 1.1.66
title: Graph. Message, user part 1
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

// Read about the Facebook RAML API at https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/contracts

API.createClient('client', '/apiplatform/repository/public/organizations/30/apis/7965/versions/8129/definition');

```

```javascript

API.authenticate(client,"oauth_2_0",{

  clientId : CLIENT_ID,

  clientSecret : CLIENT_SECRET,

  scopes: [

    "user_groups", "read_stream", "read_mailbox", "user_interests", "user_likes", "publish_actions", "user_photos", "user_games_activity", "manage_pages"

  ]

})

```

```javascript

ACCESS_TOKEN = $4.accessToken//access the 'accessToken' field of the response of the API.authenticate() cell

```



Retrieve some page's ID.



```javascript

accountsResponse = client.user_id("me").accounts.get()

PAGE_ID = accountsResponse.body.data[0].id

```



Retrieve a list of user's groups.



```javascript

groupsResponse = client.user_id("me").groups.get()

```

```javascript

assert.equal( groupsResponse.status, 200 )

```



The posts that a person sees in their Facebook News Feed.



```javascript

homeResponse = client.user_id("me").home.get()

```

```javascript

assert.equal( homeResponse.status, 200 )

```



Retrieve a person's Facebook Messages inbox.



```javascript

inboxResponse = client.user_id("me").inbox.get()

```

```javascript

assert.equal( inboxResponse.status, 200 )

messageId = inboxResponse.body.data[0].id;

```



Retrieve a single message



```javascript

messageResponse = client.message_id(messageId).get();

```

```javascript

assert.equal( messageResponse.status, 200 )

```



The interests listed on someone's profile under Likes > Interests.



```javascript

interestsResponse = client.user_id("me").interests.get()

```

```javascript

assert.equal( interestsResponse.status, 200 )

```



The Facebook Pages that this person has 'liked'.



```javascript

likesResponse = client.user_id("me").likes.get()

```

```javascript

assert.equal( likesResponse.status, 200 )

```



You can append a page id to the edge in order to determine whether that page is liked by the root node user.



```javascript

pageResponse = client.user_id("me").likes.page_id( PAGE_ID ).get()

```

```javascript

assert.equal( pageResponse.status, 200 )

```



Return an array of Post objects



```javascript

linksResponse = client.user_id("me").links.get()

```

```javascript

assert.equal( linksResponse.status, 200 )

```



A user access token with publish_actions permission can be used to publish new posts



```javascript

linksResponse = client.user_id("me").links.post({

  "access_token" : ACCESS_TOKEN,

  "link" : "http://raml.org"

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( linksResponse.status, 200 )

```



User locations API is deprecated for versions v2.0 and higher

An array of Post and Photo objects with one additional field type



```javascript

//user locations API is deprecated for versions v2.0 and higher

//locationsResponse = client.user_id("me").locations.get()

```

```javascript

//assert.equal( locationsResponse.status, 200 )

```



The liked movies listed on someone's profile under Movies > Likes.



```javascript

moviesResponse = client.user_id("me").movies.get()

```

```javascript

assert.equal( moviesResponse.status, 200 )

```



The liked music artists listed on someone's profile under Music > Likes.



```javascript

musicResponse = client.user_id("me").music.get()

```

```javascript

assert.equal( musicResponse.status, 200 )

```



An array of Note objects.



```javascript

//notes API is deprecated for versions v2.0 and higher

notesResponse = client.user_id("me").notes.get()

```

```javascript

assert.equal( notesResponse.status, 400 )

```



notes API is deprecated for versions v2.0 and higher

Publishing a note



```javascript

//notes API is deprecated for versions v2.0 and higher

notesResponse = client.user_id("me").notes.post({

  "access_token" : ACCESS_TOKEN,

  "message": "messageValue",

  "subject": "subjectValue"

}, {headers:{"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( notesResponse.status, 400 )

```



A person's Facebook Messages outbox.



```javascript

outboxResponse = client.user_id("me").outbox.get()

```

```javascript

assert.equal( outboxResponse.status, 200 )

```



The permissions that the app has requested from the user, and whether they were granted or declined.



```javascript

permissionsResponse = client.user_id("me").permissions.get()

```

```javascript

assert.equal( permissionsResponse.status, 200 )

```



List of all photos this person is tagged in.



```javascript

photosResponse = client.user_id("me").photos.get()

```

```javascript

assert.equal( photosResponse.status, 200 )

```



List of all photos that were published to Facebook by this person.



```javascript

uploadedResponse = client.user_id("me").photos.uploaded.get()

```

```javascript

assert.equal( uploadedResponse.status, 200 ) 

```



The Facebook Pokes that a person has received and hasn't responded to.



```javascript

pokesResponse = client.user_id("me").pokes.get()

```

```javascript

assert.equal( pokesResponse.status, 200 )

```



The user's questions.



```javascript

//questions API is deprecated for versions v2.0 and higher

questionsResponse = client.user_id("me").questions.get()

```

```javascript

assert.equal( questionsResponse.status, 400 )

```



The scores this person has received from Facebook Games that they've played.



```javascript

scoresResponse = client.user_id("me").scores.get()

```

```javascript

assert.equal( scoresResponse.status, 200 )

```



Set a score for the person.



```javascript

publishScoresResponse = client.user_id("me").scores.post({

  "access_token" : ACCESS_TOKEN,

  "score": "502"

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( publishScoresResponse.status, 200 )

```



Delete the score this person has received. You can do it from your app only.



```javascript

deleteScoresResponse = client.user_id("me").scores.delete({

  "score": "502"

})

```

```javascript

assert.equal( deleteScoresResponse.status, 200 )

```