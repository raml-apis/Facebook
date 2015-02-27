---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/portal/pages/6775/preview
apiNotebookVersion: 1.1.66
title: Graph. Video, order, review, search, request
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

    "publish_actions", "user_videos", "read_stream", "publish_stream","read_page_mailboxes", "read_mailbox", "user_groups", "user_friends ", "email"

  ]

})

```

```javascript

ACCESS_TOKEN = $4.accessToken//access the 'accessToken' field of the response of the API.authenticate() cell

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



Retrieve the videos this user has uploaded



```javascript

userVideoResponse = client.user_id("me").videos.uploaded.get()

```

```javascript

video_id = userVideoResponse.body.data[0].id

```



To read a Video, issue an HTTP GET request to /VIDEO_ID with the user_videos permission. This will return videos that the user has uploaded or has been tagged in



```javascript

videoResponse = client.video_id( video_id ).get()

```

```javascript

assert.equal( videoResponse.status, 200 )

```



You can comment on a Video by issuing an HTTP POST request to VIDEO_ID/comments with the publish_stream permission and following parameters



```javascript

commentCreateResponse = client.video_id( video_id ).comments.post({

  "access_token" : ACCESS_TOKEN,

  "message": "This test comment was created by the 'facebook' API Notebook"

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( commentCreateResponse.status, 200 )

commentId = commentCreateResponse.body.id

```



Now let's remove the test comment



```javascript

client.object_id(commentId).delete()

```



Getting the reviews of the App Dashboard



```javascript

//not supported

//reviewResponse = client.review_id( review_id ).reviews.get()

```

```javascript

//assert.equal( reviewResponse.status, 200 )

```



You can like a Video by issuing an HTTP POST request to VIDEO_ID/likes with the publish_stream permission. No parameters necessary



```javascript

likesResponse = client.video_id( video_id ).likes.post()

```

```javascript

assert.equal( likesResponse.status, 200 )

```



You can unlike a Video by issuing an HTTP DELETE request to VIDEO_ID/like with the publish_stream permission



```javascript

likesResponse = client.video_id( video_id ).likes.delete()

```

```javascript

assert.equal( likesResponse.status, 200 )

```



This reference describes the /sharedposts edge that is common to multiple Graph API nodes. The structure and operations are the same for each node.



This edge represents any posts where the original object was shared on Facebook.



```javascript

sharedpostsResponse = client.video_id( video_id ).sharedposts.get()

```

```javascript

assert.equal( sharedpostsResponse.status, 200 )

```



You can read the scores for the user and their friends for your app by issuing an HTTP GET request to /APP_ID/scores with the user access_token for that app. This returns the list of scores for the user and their friends who have a uthorized the app. You can use this api to create leaderboard for the user and their friends



```javascript

scoresResponse = client.app_id(CLIENT_ID).scores.get()

```

```javascript

assert.equal( scoresResponse.status, 200 )

```



If you issue a HTTP GET request to /ORDER_ID and if the order in question has been refunded by Facebook, then there you will see the refund_reason_code which explains why the order was refunded by Facbook with one of values below. See more information about why Facebook refunds orders in the Chargebacks and disputes doc



```javascript

//not supported

//orderResponse = client.order_id(order_id).get()

```

```javascript

//assert.equal( orderResponse.status, 200 )

```



You can update properties an existing order by issuing a HTTP POST request to /ORDER_ID with the list of parameters you wish to update



```javascript

//not supported

//updateOrderResponse = client.order_id(order_id).post({})

```

```javascript

//assert.equal( updateOrderResponse.status, 200 )

```



An array requests received by this person from the app making the API call.



```javascript

apprequestsResponse = client.user_id("me").apprequests.get()

```

```javascript

assert.equal( apprequestsResponse.status, 200 )

```



Create a request



```javascript

apprequestCreateResponse = client.user_id("me").apprequests.post({

  "access_token" : ACCESS_TOKEN,

  "message" : "This app request was created by the API Notebook"

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( apprequestCreateResponse.status, 200 )

requestId = apprequestCreateResponse.body.request

```



Retrieve a request.



```javascript

requestResponse = client.request_id(requestId).get()

```

```javascript

assert.equal( requestResponse.status, 200 )

```



Delete the request



```javascript

deleteRequestResponse = client.request_id(requestId).delete()

```

```javascript

assert.equal( deleteRequestResponse.status, 200 )

```