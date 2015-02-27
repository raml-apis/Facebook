---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/portal/pages/6778/preview
apiNotebookVersion: 1.1.66
title: Graph. Post, object, thread, user part 4
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

    "manage_pages", "user_games_activity", "user_activities", "user_photos", "user_likes", "user_events", "friends_events", "user_relationships", "read_stream", "manage_friendlists", "user_friends", "friends_relationships",

    "read_requests", "publish_actions", "publish_stream","user_photos"

  ]

})

```

```javascript

ACCESS_TOKEN = $4.accessToken//access the 'accessToken' field of the response of the API.authenticate() cell

```

```javascript

IS_ADMIN = window.confirm("Some methods in this notebook require you to be an admin or developer of your Facebook application. Press 'OK' if you are admin or developer.\n\nOtherwise press 'CANCEL'. In this case Notebook will not try executing these methods.")

```



Create a post



```javascript

postCreateResponse = client.user_id("me").feed.post({

  "access_token" : ACCESS_TOKEN,

  "message" : "Yet another API Notebook test message"

},{headers:{"Content-Type":"application/x-www-form-urlencoded"}})

postId = postCreateResponse.body.id

```



Retrieve comments on the particular post.



```javascript

postCommentsResponse = client.post_id(postId).comments.get()

```

```javascript

assert.equal( postCommentsResponse.status, 200 )

```



Write a comment.



```javascript

postCommentsCreateResponse = client.post_id(postId).comments.post({

  "access_token" : ACCESS_TOKEN,

  "message": "This comment has been created by the API Notebook"

},{headers:{"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( postCommentsCreateResponse.status, 200 )

```



Facebook Insights is a product available to all Pages and Apps on Facebook, and any domains claimed by a site developer using the Insights dashboard. This object represents a single Insights metric that is tied to another particular Graph API object (Page, Post, etc.).



```javascript

postInsightsResponse = client.post_id(postId).insights.get()

```

```javascript

assert.equal( postInsightsResponse.status, 200 )

```



A list of people who like this post.



```javascript

postLikesCreateResponse = client.post_id(postId).likes.post()

```

```javascript

assert.equal( postLikesCreateResponse.status, 200 )

```



You can unlike a post by issuing an HTTP DELETE request to the POST_ID/likes connection with the publish_stream permission



```javascript

postLikesDeleteResponse = client.post_id(postId).likes.delete()

```

```javascript

assert.equal( postLikesDeleteResponse.status, 200 )

```



You can delete a post as long as your application created the post. You delete a post by issuing an HTTP DELETE request to the POST_ID object with publish_stream permission



```javascript

postDeleteResponse = client.post_id(postId).delete()

```

```javascript

assert.equal( postDeleteResponse.status, 200 )

```



Let's create another post.



```javascript

postCreateResponse = client.user_id("me").feed.post({

  "access_token" : ACCESS_TOKEN,

  "message" : "Yet another API Notebook test message"

},{headers:{"Content-Type":"application/x-www-form-urlencoded"}})

postId = postCreateResponse.body.id

```



You can retrieve an individual object instance



```javascript

objectResponse = client.object_id(postId).get()

```

```javascript

assert.equal( objectResponse.status, 200 )

```



View comments on the object.



```javascript

objectCommentsResponse = client.object_id(postId).comments.get()

```

```javascript

assert.equal( objectCommentsResponse.status, 200 )

```



Post a comment.



```javascript

objectCommentsCreateResponse = client.object_id(postId).comments.post({

  "access_token" : ACCESS_TOKEN,

  "message": "This comment has been created by the API Notebook"

},{headers:{"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( objectCommentsCreateResponse.status, 200 )

```



Users who liked the object.



```javascript

objectLikesResponse = client.object_id(postId).likes.get()

```

```javascript

assert.equal( objectLikesResponse.status, 200 )

```



You can add new likes to any object



```javascript

objectLikesCreateResponse = client.object_id(postId).likes.post()

```

```javascript

assert.equal( objectLikesCreateResponse.status, 200 )

```



You can delete likes using this edge



```javascript

objectLikesDeleteResponse = client.object_id(postId).likes.delete()

```

```javascript

assert.equal( objectLikesDeleteResponse.status, 200 )

```



See https://developers.facebook.com/docs/graph-api/reference/v2.1/insights#metrics 



```javascript

objectInsightsMetricNameResponse = client.object_id(postId).insights.metric_name("post_impressions").get()

```

```javascript

assert.equal( objectInsightsMetricNameResponse.status, 200 )

```



This reference describes the /sharedposts edge that is common to multiple Graph API nodes. The structure and operations are the same for each node.



This edge represents any posts where the original object was shared on Facebook.



```javascript

objectSharedpostsResponse = client.object_id(postId).sharedposts.get()

```

```javascript

assert.equal( objectSharedpostsResponse.status, 200 )

```



Deleting an object by ID



```javascript

objectDeleteResponse = client.object_id(postId).delete()

```

```javascript

assert.equal( objectDeleteResponse.status, 200 )

```



Retrieve threads



```javascript

if(IS_ADMIN){

  threadsResponse = client("/me/threads").get()

}

```

```javascript

if(IS_ADMIN){

  assert.equal(threadsResponse.status,200)

  threadId = threadsResponse.body.data[0].id

}

```



Retrieve a single thread



```javascript

if(IS_ADMIN){

  threadResponse = client.thread_id(threadId).get()

}

```

```javascript

if(IS_ADMIN){

  assert.equal( threadResponse.status, 200 )

}

```



List of former thread participants who have unsubscribed from the thread



```javascript

if(IS_ADMIN){

  formerParticipantsResponse = client.thread_id(threadId).former_participants.get()

}

```

```javascript

if(IS_ADMIN){

  assert.equal( formerParticipantsResponse.status, 200 )

}

```



List of the message objects contained in this thread



```javascript

if(IS_ADMIN){

  messagesResponse = client.thread_id(threadId).messages.get()

}

```

```javascript

if(IS_ADMIN){

	assert.equal( messagesResponse.status, 200 )

}

```



List of the thread participants



```javascript

if(IS_ADMIN){

  participantsResponse = client.thread_id(threadId).participants.get()

}

```

```javascript

if(IS_ADMIN){

  assert.equal( participantsResponse.status, 200 )

}

```



List of participants who have sent a message in the thread



```javascript

if(IS_ADMIN){

  sendersResponse = client.thread_id(threadId).senders.get()

}

```

```javascript

if(IS_ADMIN){

  assert.equal( sendersResponse.status, 200 )

}

```



Thread tags



```javascript

//not suppported

//tagsResponse = client.thread_id(threadId).tags.get()

```

```javascript

//assert.equal( tagsResponse.status, 200 )

```



An array of Notification objects.



```javascript

//not supported

//notificationsResponse = client.user_id("me").notifications.get()

```

```javascript

//assert.equal( notificationsResponse.status, 200 )

```



The unread Facebook notifications that a person has.



```javascript

//not supported

// publishingNotificationsResponse = client.user_id( "me" ).notifications.post({

//   "access_token": APP_ACCESS_TOKEN,

//   "href" : "/notification.test",

//   "template" : "This+is+a+test+message"

// }, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

//assert.equal( publishingNotificationsResponse.status, 200 )

```



The Facebook Payments orders this person placed with an app.



```javascript

//not supported

// payment_transactionsResponse = client.user_id( "me" ).payment_transactions.get({

//   "access_token": APP_ACCESS_TOKEN

// })

```

```javascript

//assert.equal( payment_transactionsResponse.status, 200 )

```