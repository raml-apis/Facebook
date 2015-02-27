---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/portal/pages/6769/preview
apiNotebookVersion: 1.1.66
title: Graph. Page, conversations, milestone, offer
---

```javascript

load('https://github.com/chaijs/chai/releases/download/1.9.0/chai.js')

```



See http://chaijs.com/guide/styles/ for assertion styles



```javascript

assert = chai.assert

```



This notebook requires data which can only be obtained in another notebook. Before running it you must run the auxiliary notebook: https://anypoint.mulesoft.com/apiplatform/popular/#/portals/apis/7965/versions/8129/pages/7046 in order to obtain PAGE_ACCESS_TOKEN.



In this notebook you must use the same PAGE_ID as in the auxiliary notebook. 



```javascript

CLIENT_ID = prompt("Please, enter 'APP ID' of your facebook application.")

CLIENT_SECRET = prompt("Please, enter 'APP SECRET' of your facebook application.")

```

```javascript

PAGE_ID = prompt("Please, enter ID of a page you wish to work with. This page must be created by you and it must be involved into conversation.")

```

```javascript

// Read about the Facebook RAML API at https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/contracts

API.createClient('client', '/apiplatform/repository/public/organizations/30/apis/7965/versions/8129/definition');

```

```javascript

PAGE_ACCESS_TOKEN = prompt("Please, enter PAGE_ACCESS_TOKEN which has been obtained in the auxiliary notebook.")

```

```javascript

function getDate(){

  var date = new Date()

  var h = date.getHours()

  var min = date.getMinutes()

  var m = date.getMonth() + 1

  var d = date.getDate()

  var y = date.getFullYear()

  var s = date.getSeconds()

  var result = m+'.'+d+'.'+y+'/'+h+':'+min+':'+s

  return result

}

```



Retrieve app access token.



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



Create a test user.



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



Retreive a page



```javascript

pageResponse = client.page_id(PAGE_ID).get({access_token:PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( pageResponse.status, 200 )

```



This returns an array of User object



```javascript

adminsResponse = client.page_id(PAGE_ID).admins.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( adminsResponse.status, 200 )

adminId = adminsResponse.body.data[0].id

```



You can also append a person's user id or username to the edge in order to directly determine whether that person is an admin of the page

If they are an admin, the response will contain the User object for the user along with the fields above. If they are not, there will be an empty dataset returned.



```javascript

userIdResponse = client.page_id(PAGE_ID).admins.user_id(adminId).get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( userIdResponse.status, 200 )

```



Reading this endpoint returns an array of Album objects with the same fields as that node



```javascript

albumsResponse = client.page_id(PAGE_ID).albums.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( albumsResponse.status, 200 )

```



You can create an empty album of a page by issuing an HTTP Post request



```javascript

albumsCreateResponse = client.page_id(PAGE_ID).albums.post({

  "access_token" : PAGE_ACCESS_TOKEN,

  "name" : "API Notebook Test Album",

  "message" : "Running the 'facebook' API Notebook",

  "privacy" : {

    value : "EVERYONE"

  }

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( albumsCreateResponse.status, 200 )

```



A list of people blocked from viewing this Page



```javascript

blockedResponse = client.page_id(PAGE_ID).blocked.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( blockedResponse.status, 200 )

```

```javascript

BLOCKED_USER_ID = prompt("Now we are about to test blocking user from a page. Please, enter ID of some real user. Do not enter ID of page admin.\n\nPress 'Cancel' if you do not want to test this API.")

```



You can publish to this edge to block people from a Page



```javascript

if(BLOCKED_USER_ID){

  blockedCreateResponse = client.page_id(PAGE_ID).blocked.post({

    "access_token" : PAGE_ACCESS_TOKEN,

    "user": BLOCKED_USER_ID

  }, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

}

```

```javascript

if(BLOCKED_USER_ID){

  assert.equal( blockedCreateResponse.status, 200 )

}

```



You can also append a person's user id or username to the edge in order to directly determine whether that person is blocked by the page.

If they are blocked, the response will contain the User object for that person. If they are not, there will be an empty dataset returned.



```javascript

if(BLOCKED_USER_ID){

  userBlockedResponse = client.page_id(PAGE_ID).blocked.user_id(BLOCKED_USER_ID).get({"access_token":PAGE_ACCESS_TOKEN})

}

```

```javascript

if(BLOCKED_USER_ID){

  assert.equal( userBlockedResponse.status, 200 )

}

```



You can delete on this edge to remove someone from the blocked list for that Page



```javascript

if(BLOCKED_USER_ID){

  blockedDeleteResponse = client.page_id(PAGE_ID).blocked.delete(null,{query:{

    "access_token":PAGE_ACCESS_TOKEN,

    "user": BLOCKED_USER_ID

  }})

}

```

```javascript
if(BLOCKED_USER_ID){
	assert.equal( blockedDeleteResponse.status, 200 )
}
```



The Facebook Messages conversations that this Page is involved in.

Reading this endpoint returns an array of Conversation objects with the same fields as that node.



```javascript

conversationsResponse = client.page_id(PAGE_ID).conversations.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( conversationsResponse.status, 200 )

conversation_id = conversationsResponse.body.data[0].id

```



Reading of conversation.

Permissions

A page access token with read_page_mailboxes permission can be used to view any conversation that Page is involved with.

A user access token with read_mailbox permission can be used to view any conversation that person is involved with if they are a developer of the app making the request.



```javascript

conversationResponse = client.conversation_id( conversation_id ).get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( conversationResponse.status, 200 )

```



Pages can only reply to a message - they cannot initiate a conversation. Also, a Page can only respond twice to a particular message, the other party will have to respond before they can reply again



```javascript

messagesResponse = client.conversation_id( conversation_id ).messages.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( messagesResponse.status, 200 )

```



Reading this endpoint returns an array of Message objects



```javascript

messageCreateResponse = client.conversation_id( conversation_id ).messages.post({

  "access_token" : PAGE_ACCESS_TOKEN,

  "message": "API Notebook test message"

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( messageCreateResponse.status, 200 )

```



The events this Page has created.



```javascript

eventsResponse = client.page_id(PAGE_ID).events.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( eventsResponse.status, 200 )

```



Return an array of Post objects



```javascript

feedResponse = client.page_id(PAGE_ID).feed.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( feedResponse.status, 200 )

```



You can create links, status messages, or posts by using this edge



```javascript

feedCreateResponse = client.page_id(PAGE_ID).feed.post({

  "access_token" : PAGE_ACCESS_TOKEN,

  "message" : "This post was created by the API Notebook " + getDate()

},{headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( feedCreateResponse.status, 200 )

```



Reading this endpoint returns an array of Page objects representing each of the child brands



```javascript

globalBrandChildrenResponse = client.page_id(PAGE_ID).global_brand_children.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( globalBrandChildrenResponse.status, 200 )

```



Insights information for this Post, If this Post is from a Facebook Page.

Permissions a valid access_token with read_insights permission for a user authorized to view the Page's insights.

Return an array of Insights objects. See the Insights documentation for more information.



```javascript

insightsResponse = client.page_id(PAGE_ID).insights.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( insightsResponse.status, 200 )

```



The feed of links published by this page.

Return an array of Link objects.



```javascript

linksResponse = client.page_id(PAGE_ID).links.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( linksResponse.status, 200 )

```



The list of Pages that represent different real-world locations of a parent business Page (for example - a chain of restaurants).

An array of Page objects, each representing an individual location for the business.



```javascript

locationsResponse = client.page_id(PAGE_ID).locations.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( locationsResponse.status, 200 )

```



You can add or update an existing location-based Page to this list by publishing on this edge



```javascript

//not supported

// locationsCreateResponse = client.page_id(PAGE_ID).locations.post({

//   "access_token" : PAGE_ACCESS_TOKEN,

//   "main_page_id" : PAGE_ID,

//   "store_number" : "12345",

//   "location_page_id" : "111948542155151"

// },{headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

//assert.equal( locationsCreateResponse.status, 200 )

```



You can remove a location page from a parent's list of locations by deleting on this edge



```javascript

//not supported

//locationsDeleteResponse = client.page_id(PAGE_ID).locations.delete()

```

```javascript

//assert.equal( locationsDeleteResponse.status, 200 )

```



The milestones posted by a Facebook Page.

An array of Milestone objects.



```javascript

milestonesResponse = client.page_id(PAGE_ID).milestones.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( milestonesResponse.status, 200 )

```



You can create Page milestones by using this edge



```javascript

milestonesCreateResponse = client.page_id(PAGE_ID).milestones.post({

  "access_token" : PAGE_ACCESS_TOKEN,

  "title" : "Final Episode Aired",

  "description" : "The big finale was today!",

  "start_time" : "2013-09-29T21:00:00+0500"

},{headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( milestonesCreateResponse.status, 200 )

milestoneId = milestonesCreateResponse.body.id

```



You can retrieve Page milestones by it's id



```javascript

milestoneResponse = client.milestone_id(milestoneId).get({"access_token" : PAGE_ACCESS_TOKEN});

```

```javascript

assert.equal( milestoneResponse.status, 200 )

```



You can delete Page milestones by it's id



```javascript

milestoneDeleteResponse = client.milestone_id(milestoneId).delete(null,{query:{"access_token" : PAGE_ACCESS_TOKEN}});

```

```javascript

assert.equal( milestoneDeleteResponse.status, 200 )

```



Return an array of Note objects



```javascript

//notes API is deprecated for versions v2.0 and higher

notesResponse = client.page_id(PAGE_ID).notes.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( notesResponse.status, 400 )

```



Publishing a note



```javascript

//notes API is deprecated for versions v2.0 and higher

 noteCreateResponse = client.page_id(PAGE_ID).notes.post({

   "access_token" : PAGE_ACCESS_TOKEN,

   "message" : "messageValue"

 },{headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( noteCreateResponse.status, 400 )

```



Return an array of Offer objects



```javascript

offersResponse = client.page_id(PAGE_ID).offers.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( offersResponse.status, 200 )

```



You can create offers by using this edge



```javascript

//not supported

// offersCreateResponse = client.page_id(PAGE_ID).offers.post({

//   "access_token" : PAGE_ACCESS_TOKEN,

//   "title" : "Special Christmas Offer",

//   "expiration_time" : "2014-11-24T23:59:59+0000"

// },{headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

// assert.equal( offersCreateResponse.status, 200 )

// offerId = offersCreateResponse.body.id

```



Retrieve an offer



```javascript

//not supported

//offerResponse = client.offer_id(offerId).get(null,{query:{access_token:PAGE_ACCESS_TOKEN}})

```

```javascript

//assert.equal(offerResponse.status,200)

```



Retrieve page's profile picture.



```javascript

pictureResponse = client.page_id(PAGE_ID).picture.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( pictureResponse.status, 200 )

```



Publiahsh page's profile picture.



```javascript

// pictureCreateResponse = client.page_id(PAGE_ID).picture.post({

//   "access_token" : PAGE_ACCESS_TOKEN,

//   "source" : pictureResponse.body

// },{headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

//assert.equal( pictureCreateResponse.status, 200 )

```



Shows all photos this page is tagged in.

Return an array of Photo objects.



```javascript

photosResponse = client.page_id(PAGE_ID).photos.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( photosResponse.status, 200 )

```



Publishing photos to Facebook



```javascript

photosCreateResponse = client.page_id(PAGE_ID).photos.post({

  "access_token" : PAGE_ACCESS_TOKEN,

  "url" : "https://api-notebook.anypoint.mulesoft.com/images/notebook-graphic.png"

},{headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( photosCreateResponse.status, 200 )

```



Shows all photos that were published to Facebook by this page



```javascript

uploadedResponse = client.page_id(PAGE_ID).photos.uploaded.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( uploadedResponse.status, 200 )

```



Shows only the posts that were published by this page.

Return an array of Post objects.



```javascript

postsResponse = client.page_id(PAGE_ID).posts.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( postsResponse.status, 200 )

```



Shows only the posts that can be boosted (includes unpublished and scheduled posts)



```javascript

promotablePostsResponse = client.page_id(PAGE_ID).promotable_posts.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( promotablePostsResponse.status, 200 )

```



Return an array of Question objects



```javascript

//(#12) questions API is deprecated for versions v2.0 and higher

questionsResponse = client.page_id(PAGE_ID).questions.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( questionsResponse.status, 400 )

```



Reading a settings



```javascript

settingsResponse = client.page_id(PAGE_ID).settings.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( settingsResponse.status, 200 )

```



You can adjust Page settings by using this edge



```javascript

settingsCreateResponse = client.page_id(PAGE_ID).settings.post({

  "access_token" : PAGE_ACCESS_TOKEN,

  "setting" : "USERS_CAN_TAG_PHOTOS",

  "value" : 0

},{headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( settingsCreateResponse.status, 200 )

```



Permissions

No access token is required to view any publicly shared statuses.

A user access token is required to retrieve statuses visible to that person.

A page access token is required to retrieve any other statuses.

Fields

An array of Status Message objects.



```javascript

statusesResponse = client.page_id(PAGE_ID).statuses.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( statusesResponse.status, 200 )

```



Page tabs



```javascript

tabsResponse = client.page_id(PAGE_ID).tabs.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( tabsResponse.status, 200 )

```



You can add apps as a tab using this edge



```javascript

tabsCreateResponse = client.page_id(PAGE_ID).tabs.post({

  "access_token" : PAGE_ACCESS_TOKEN,

  "app_id" : CLIENT_ID

},{headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( tabsCreateResponse.status, 200 )

```



You can update tabs by using this edge ({app_id} is the ID of the app in the tab that you want to update



```javascript

//not supported

// tabUpdateResponse = client.page_id(PAGE_ID).tabs.app_(CLIENT_ID).post({

//   "access_token" : PAGE_ACCESS_TOKEN

// },{headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

//assert.equal( tabUpdateResponse.status, 200 )

```



You can remove tabs using this edge



```javascript

appAppIdDeleteResponse = client.page_id(PAGE_ID).tabs.app_(CLIENT_ID).delete(null,{query:{"access_token":PAGE_ACCESS_TOKEN}})

```

```javascript

assert.equal( appAppIdDeleteResponse.status, 200 )

```



Shows only the posts that this page was tagged in



```javascript

taggedResponse = client.page_id(PAGE_ID).tagged.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( taggedResponse.status, 200 )

```



Shows all videos this page is tagged in



```javascript

videosResponse = client.page_id(PAGE_ID).videos.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( videosResponse.status, 200 )

```



Shows all videos that were published to Facebook by this page.

Return an array of Video objects.



```javascript

uploadedResponse = client.page_id(PAGE_ID).videos.uploaded.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( uploadedResponse.status, 200 )

```



Reading a ranings



```javascript

ratingsResponse = client.page_id(PAGE_ID).ratings.get({"access_token":PAGE_ACCESS_TOKEN})

```

```javascript

assert.equal( ratingsResponse.status, 200 )

```



Delete the test user



```javascript

client.TEST_USER_ID(TEST_USER_ID).delete(null,{query:{"access_token": APP_ACCESS_TOKEN}})

```