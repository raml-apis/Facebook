---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/organizations/52560d3f-c37a-409d-9887-79e0a9a9ecff/dashboard/apis/15451/versions/27787/portal/pages/41352/edit
apiNotebookVersion: 1.1.69
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

API.createClient('client', '#REF_TAG_DEFENITION');

```

```javascript

API.authenticate(client,"oauth_2_0",{

  clientId : CLIENT_ID,

  clientSecret : CLIENT_SECRET,

  scopes: [

    "publish_actions"

  ]

})

```

```javascript

API.createClient('graphClient', '#REF_TAG_DEFENITION_Facebook:');

```

```javascript
API.authenticate(graphClient,"oauth_2_0",{

  clientId: CLIENT_ID,

  clientSecret: CLIENT_SECRET,

  scopes: [

     "user_events", "user_groups", "user_managed_groups"

  ]

})
```

```javascript
ACCESS_TOKEN = $6.accessToken
```

```javascript

appTokenResponse = graphClient("/oauth/access_token").get({

  client_id : CLIENT_ID,

  client_secret : CLIENT_SECRET,

  grant_type : "client_credentials",

  scopes: [

    "manage_pages", "user_events", "publish_actions", "user_managed_groups"

  ]

})
```

```javascript
GRAPH_APP_ACCESS_TOKEN = appTokenResponse.body.substring(appTokenResponse.body.indexOf("=")+1)
```

```javascript
formData = new FormData()
formData.append("file_url","http://r19---sn-aigllns7.googlevideo.com/videoplayback?mn=sn-aigllns7&mm=31&dur=0.000&id=o-AMhJ362XQcKNlGzoON7EHTLn-hc0qnRPoBmCS3ByJeHL&mv=m&source=youtube&ms=au&lmt=1429926415186769&key=yt5&ip=2a02%3A2498%3Ae000%3A85%3A45%3A%3A2&fexp=901816%2C9405191%2C9407141%2C9408142%2C9408420%2C9408710%2C9408859%2C9409208%2C9412469%2C9414929%2C9414967%2C9415171%2C9415430%2C9415435%2C9416126%2C952640&pl=32&sver=3&expire=1436188241&initcwndbps=2322500&upn=IWiXY_rDiGE&signature=5236B6691A8EBE1ACBC8E764FA2D3AF1100E937A.3B918155B0E41AC43F9977B6983272EF918FD12A&mime=video%2Fwebm&sparams=dur%2Cid%2Cinitcwndbps%2Cip%2Cipbits%2Citag%2Clmt%2Cmime%2Cmm%2Cmn%2Cms%2Cmv%2Cnh%2Cpl%2Cratebypass%2Csource%2Cupn%2Cexpire&ipbits=0&ratebypass=yes&itag=43&mt=1436166536&nh=IgpwcjAyLmxocjE0KgkxMjcuMC4wLjE&title=Dataloader+Delete")
```

Publish video to current user account.

```javascript
userVideosCreateResponse = client.user_id("me").videos.post(formData)
```

```javascript
assert.equal( userVideosCreateResponse.status, 200 )
```

Event ID.

```javascript
events = graphClient.user_id("me").events.created.get();
eventId = events.body.data[0].id;
```

Publish video to event.

```javascript
eventVideosCreateResponse = client.event_id(eventId).videos.post(formData)
```

```javascript
assert.equal( eventVideosCreateResponse.status, 200 )
```

Current user page ID.

```javascript
USER_PAGE_ID = graphClient.user_id("me").get().body.id
```

Publish video to page.

```javascript
pageVideosCreateResponse = client.page_id(USER_PAGE_ID).videos.post(formData)
```

```javascript
assert.equal( pageVideosCreateResponse.status, 200 )
```

Retrive group ID.

```javascript
groupsCreateResponse = graphClient.app_id(CLIENT_ID).groups.post({

  "access_token" : GRAPH_APP_ACCESS_TOKEN,

  "name": "API Notebook Video Test Group"

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}});
groupId = groupsCreateResponse.body.id
```

Add user to the group.

```javascript
userAddResponse = graphClient.group_id(groupId).members.USER_ID(USER_PAGE_ID).post({

  "access_token": GRAPH_APP_ACCESS_TOKEN

},{headers: {"Content-Type":"application/x-www-form-urlencoded"}});
```

Publish video to group.

```javascript
groupVideosCreateResponse = client.group_id(groupId).videos.post(formData)
```

```javascript
assert.equal( groupVideosCreateResponse.status, 200 )
```

Delete group member.

```javascript
graphClient.group_id(groupId).members.USER_ID(USER_PAGE_ID).delete({

  "access_token" : GRAPH_APP_ACCESS_TOKEN

});
```

Delete group.

```javascript
groupDeleteResponse = graphClient.app_id(CLIENT_ID).groups.GROUP_ID(groupId).delete({

  "access_token" : GRAPH_APP_ACCESS_TOKEN

})
```

Retrieve the videos this user has uploaded



```javascript

userVideoResponse = graphClient.user_id("me").videos.uploaded.get()

```

```javascript

video_id = userVideoResponse.body.data[0].id

```



To read a Video, issue an HTTP GET request to /VIDEO_ID with the user_videos permission. This will return videos that the user has uploaded or has been tagged in



```javascript

videoResponse = graphClient.video_id( video_id ).get()

```

```javascript

assert.equal( videoResponse.status, 200 )

```



You can comment on a Video by issuing an HTTP POST request to VIDEO_ID/comments with the publish_stream permission and following parameters



```javascript

commentCreateResponse = graphClient.video_id( video_id ).comments.post({

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

graphClient.object_id(commentId).delete()

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

likesResponse = graphClient.video_id( video_id ).likes.post()

```

```javascript

assert.equal( likesResponse.status, 200 )

```



You can unlike a Video by issuing an HTTP DELETE request to VIDEO_ID/like with the publish_stream permission



```javascript

likesResponse = graphClient.video_id( video_id ).likes.delete()

```

```javascript

assert.equal( likesResponse.status, 200 )

```



This reference describes the /sharedposts edge that is common to multiple Graph API nodes. The structure and operations are the same for each node.



This edge represents any posts where the original object was shared on Facebook.



```javascript

sharedpostsResponse = graphClient.video_id( video_id ).sharedposts.get()

```

```javascript

assert.equal( sharedpostsResponse.status, 200 )

```



You can read the scores for the user and their friends for your app by issuing an HTTP GET request to /APP_ID/scores with the user access_token for that app. This returns the list of scores for the user and their friends who have a uthorized the app. You can use this api to create leaderboard for the user and their friends



```javascript

scoresResponse = graphClient.app_id(CLIENT_ID).scores.get()

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

apprequestsResponse = graphClient.user_id("me").apprequests.get()

```

```javascript

assert.equal( apprequestsResponse.status, 200 )

```



Create a request



```javascript

apprequestCreateResponse = graphClient.user_id("me").apprequests.post({

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

requestResponse = graphClient.request_id(requestId).get()

```

```javascript

assert.equal( requestResponse.status, 200 )

```



Delete the request



```javascript

deleteRequestResponse = graphClient.request_id(requestId).delete()

```

```javascript

assert.equal( deleteRequestResponse.status, 200 )

```