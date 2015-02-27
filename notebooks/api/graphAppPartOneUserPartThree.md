---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/portal/pages/6776/preview
apiNotebookVersion: 1.1.66
title: Graph. App part 1, user part 3
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

    "user_groups", "read_stream"

  ]

})

```

```javascript

ACCESS_TOKEN = $4.accessToken

```

```javascript

IS_ADMIN = window.confirm("Some methods in this notebook require you to be an admin of your Facebook application. Press 'OK' if you are admin.\n\nOtherwise press 'CANCEL'. In this case Notebook will not try executing these methods.")

```



Retreive app access token



```javascript

appTokenResponse = client("/oauth/access_token").get({

  client_id : CLIENT_ID,

  client_secret : CLIENT_SECRET,

  grant_type : "client_credentials"

})

```

```javascript

APP_ACCESS_TOKEN = appTokenResponse.body.substring(appTokenResponse.body.indexOf("=")+1)

```



Return an array of status update posts published by the user themselves



```javascript

statusesResponse = client.user_id("me").statuses.get()

```

```javascript

assert.equal( statusesResponse.status, 200 )

```



This is a duplicate of the /feeds edge which only shows posts in which the user is tagged.



```javascript

taggedResponse = client.user_id("me").tagged.get()

```

```javascript

assert.equal( taggedResponse.status, 200 )

```



A list of tags of this person at a place in a photo, video, post, status or link.



```javascript

taggedPlacesResponse = client.user_id("me").tagged_places.get()

```

```javascript

assert.equal( taggedPlacesResponse.status, 200 )

```



he liked TV shows listed on someone's profile under TV Shows > Likes. 



```javascript

televisionResponse = client.user_id("me").television.get()

```

```javascript

assert.equal( televisionResponse.status, 200 )

```



 Shows all videos this person is tagged in.



```javascript

videosResponse = client.user_id("me").videos.get()

```

```javascript

assert.equal( videosResponse.status, 200 )

```



Upload a video. Videos must be encoded as multipart/form-data and published to graph-video.facebook.com instead of the regular Graph API URL



```javascript

//Not supported

// videosResponse = client.user_id("me").videos.post({

//   "source": "VIDEO DATA"

// })

```

```javascript

//assert.equal( videosResponse.status, 200 )

```



Shows all videos that were published to Facebook by this person.



```javascript

uploadedResponse = client.user_id("me").videos.uploaded.get()

```

```javascript

assert.equal( uploadedResponse.status, 200 )

```



An array of User objects each representing a mutual friend.



```javascript

//user/mutual_friends is deprecated for versions v2.0 and higher

anotherUserId = client.user_id("me").get().body.id

mutualfriendsResponse = client.user_id("me").mutualfriends.user_id_b(anotherUserId).get()

```

```javascript

assert.equal( mutualfriendsResponse.status, 400 )

```



Return an array of Post objects



```javascript

postsResponse = client.user_id("me").posts.get()

```

```javascript

assert.equal( postsResponse.status, 200 )

```



The user's profile picture.



```javascript

pictureResponse = client.user_id("me").picture.get()

```

```javascript

assert.equal( pictureResponse.status, 200 )

```



Create a test user



```javascript

testUserResponse = client("/{app_id}/accounts/test-users",{app_id:CLIENT_ID}).post({

  "access_token": APP_ACCESS_TOKEN,

  "password" : "!@#RAML#@!",

  "name" : "John Smith test"

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

TEST_USER_ID = testUserResponse.body.id

```



You can define a role for a user by issuing an HTTP POST request to APP_ID/roles with a user access token for an administrator of the app



```javascript

if(IS_ADMIN){

  rolesCreateResponse = client.app_id(CLIENT_ID).roles.post({

    "access_token" : ACCESS_TOKEN,

    "role" : "developers",

    "user" : TEST_USER_ID

  }, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

}

```

```javascript

if(IS_ADMIN){

  assert.equal( rolesCreateResponse.status, 200 )

}

```



You can remove a defined user role from an app by issuing an HTTP DELETE request to /APP_ID/roles with a user access token for an administrator of the app



```javascript

if(IS_ADMIN){

  rolesDeleteResponse = client.app_id(CLIENT_ID).roles.delete(null, {query:{

    access_token:APP_ACCESS_TOKEN,

    "user" : TEST_USER_ID

  }})

}

```

```javascript

if(IS_ADMIN){

  assert.equal( rolesDeleteResponse.status, 200 )

}

```



Gets usage statistics for the static resources used by your Canvas IFrame application. These statistics are collected for the purpose of flushing some of them to the browser early, to improve application performance



```javascript

if(IS_ADMIN){

  staticresourcesResponse = client.app_id(CLIENT_ID).staticresources.get()

}

```

```javascript

if(IS_ADMIN){

  assert.equal( staticresourcesResponse.status, 200 )

}

```