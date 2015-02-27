---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/portal/pages/6777/preview
apiNotebookVersion: 1.1.66
title: Graph. App part 2, group part 1
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

USER_ID = prompt("Please, enter some 'user_id' which belongs to a user of your Facebook application.")

```

```javascript

ACHIEVEMENT_URL = prompt("Please, enter URL which points to a valid achievement definition.")

```

```javascript

// Read about the Facebook RAML API at https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/contracts

API.createClient('client', '/apiplatform/repository/public/organizations/30/apis/7965/versions/8129/definition');

```



Retrieve app access token



```javascript

appTokenResponse = client("/oauth/access_token").get({

  client_id : CLIENT_ID,

  client_secret : CLIENT_SECRET,

  grant_type : "client_credentials",

  scopes: [

    "manage_pages", "user_games_activity", "user_activities", "user_photos", "user_likes", "user_events", "friends_events", "user_relationships", "read_stream", "manage_friendlists", "user_friends", "friends_relationships",

    "read_requests", "publish_actions", "publish_stream","user_photos"

  ]

})

```

```javascript

APP_ACCESS_TOKEN = appTokenResponse.body.substring(appTokenResponse.body.indexOf("=")+1);

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



Retrieve an application.



```javascript

appIdResponse = client.app_id(CLIENT_ID).get({access_token:APP_ACCESS_TOKEN})

```

```javascript

assert.equal( appIdResponse.status, 200 )

```



Update application settings.



```javascript

appIdCreateResponse = client.app_id(CLIENT_ID).post({

  "access_token" : APP_ACCESS_TOKEN

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( appIdCreateResponse.status, 200 )

```



You can get all achievements for an app by issuing an HTTP GET request to /APP_ID/achievements with an app access_token. This will return an array of achievement objects



```javascript

achievementsResponse = client.app_id(CLIENT_ID).achievements.get({access_token:APP_ACCESS_TOKEN})

```

```javascript

assert.equal( achievementsResponse.status, 200 )

```



You can register an achievement for a game by issuing an HTTP POST to APP_ID/achievements with an app access_token

A developer may update the custom properties of an achievement registration (e.g. display_order) by re-POSTing with the same achievement parameter value, but with an updated custom registration property value.



```javascript

achievementsCreateResponse = client.app_id(CLIENT_ID).achievements.post({

  "access_token" : APP_ACCESS_TOKEN,

  "achievement": ACHIEVEMENT_URL

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( achievementsCreateResponse.status, 200 )

```



You can un-register an achievement for a game by issuing an HTTP DELETE request to /APP_ID/achievements with an app access_token



```javascript

achievementsDeleteResponse = client.app_id(CLIENT_ID).achievements.delete(null,{query:{

  "access_token" : APP_ACCESS_TOKEN,

  "achievement": ACHIEVEMENT_URL

}})

```

```javascript

assert.equal( achievementsDeleteResponse.status, 200 )

```



You can retrieve a list of banned users for an app by issuing an HTTP GET request to APP_ID/banned with an application access token (i.e. a token created using the app secret, not an application Page access token as described above)



```javascript

bannedResponse = client.app_id(CLIENT_ID).banned.get({

  "access_token": APP_ACCESS_TOKEN

})

```

```javascript

assert.equal( bannedResponse.status, 200 )

```



You can ban a user for an app by issuing an HTTP POST request to APP_ID/banned?uid=USER_ID1,USER_ID2 with an application access token



```javascript

bannedCreateResponse = client.app_id(CLIENT_ID).banned.post({

  "access_token" : APP_ACCESS_TOKEN,

  "uid" : TEST_USER_ID

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( bannedCreateResponse.status, 200 )

```



You can test if a given user is banned for an app by issuing an HTTP GET request to APP_ID/banned/USER_ID with an application access token (i.e. a token created using the app secret, not an application Page access token as described above)



```javascript

USERIDResponse = client.app_id(CLIENT_ID).banned.USER_ID(TEST_USER_ID).get({access_token:APP_ACCESS_TOKEN})

```

```javascript

assert.equal( USERIDResponse.status, 200 )

```



Applications can get all the groups that belong to an application by issuing a GET request to /APP_ID/groups with an app access_token



```javascript

groupsResponse = client.app_id(CLIENT_ID).groups.get({

  "access_token": APP_ACCESS_TOKEN

})

```

```javascript

assert.equal( groupsResponse.status, 200 )

```



Create a group. Applications create a group issuing a POST request to /APP_ID/groups with an app access_token. Optionally, a user can be set as an admin of a group with the manage_groups permission.



```javascript

groupsCreateResponse = client.app_id(CLIENT_ID).groups.post({

  "access_token" : APP_ACCESS_TOKEN,

  "name": "API Notebook Test Group"

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```

```javascript

assert.equal( groupsCreateResponse.status, 200 )

groupId = groupsCreateResponse.body.id

```



Applications can invite users to a group by issuing a POST request to /GROUP_ID/members/USER_ID with an app access_token.

Note that user being invited must be a user of the application. The user will be sent a notification saying that they have been invited to the group. The notification will take them to the group page. Users can only be invited once. Subsequent invites will fail.



```javascript

userAddResponse = client.group_id(groupId).members.USER_ID(USER_ID).post({

  "access_token": APP_ACCESS_TOKEN

},{headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```

```javascript

assert.equal(userAddResponse.status, 200);

```



A member of a group can be promoted to admin by issuing a POST request to /GROUP_ID/admins/USER_ID with an app access_token. The user must be a member of the group. The user will receive a notification that they have been made an admin for the group



```javascript

adminAddResponse = client.group_id(groupId).admins.USER_ID(USER_ID).post({

  "access_token": APP_ACCESS_TOKEN

},{headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```

```javascript

assert.equal(adminAddResponse.status, 200);

```



An admin of a group can be demoted to user by issuing a DELETE request to /GROUP_ID/admins/USER_ID with an app access_token



```javascript

adminRemoveResponse = client.group_id(groupId).admins.USER_ID(USER_ID).delete(null,{query:{"access_token": APP_ACCESS_TOKEN}});

```

```javascript

assert.equal(adminRemoveResponse.status, 200);

```



Users can be removed from a group by issuing a DELETE request to /GROUP_ID/members/USER_ID with an app access_token



```javascript

userRemoveResponse = client.group_id(groupId).members.USER_ID(USER_ID).delete(null,{query:{"access_token": APP_ACCESS_TOKEN}});

```

```javascript

assert.equal(userRemoveResponse.status, 200);

```



Applications delete a group issuing a DELETE request to /APP_ID/groups/GROUP_ID with an app access_token. If the group has no members, the group will be irreversibly deleted. If the group has members, the group will no longer be associated with the application and become a normal Facebook Group



```javascript

groupDeleteResponse = client.app_id(CLIENT_ID).groups.GROUP_ID(groupId).delete({}, {query:{

  "access_token": APP_ACCESS_TOKEN

}})

```

```javascript

assert.equal( groupDeleteResponse.status, 200 )

```



To get the list of currencies mapped to your application, issue an HTTP GET request to APP_ID/payment_currencies with an app access token. The first currency listed will be treated as your app's default currency



```javascript

//not supported

//paymentCurrenciesResponse = client.app_id(CLIENT_ID).payment_currencies.get({access_token:APP_ACCESS_TOKEN})

```

```javascript

//assert.equal( paymentCurrenciesResponse.status, 200 )

```



To map an Open Graph currency object to your app, issue an HTTP POST request to APP_ID/payment_currencies with an app access token



```javascript

//not supported

// paymentCurrenciesCreateResponse = client.app_id(CLIENT_ID).payment_currencies.post({

//   "access_token" : APP_ACCESS_TOKEN,

//   "currency_url": "currency_urlValue"

// }, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

//assert.equal( paymentCurrenciesCreateResponse.status, 200 )

```



To remove a currency association from your app, make an HTTP DELETE request to APP_ID/payment_currencies with an app access token



```javascript

//not supported

// paymentCurrenciesDeleteResponse = client.app_id(CLIENT_ID).payment_currencies.delete(null, {

//   "access_token": APP_ACCESS_TOKEN,

//   "currency_url": "currency_urlValue"

// })

```

```javascript

//assert.equal( paymentCurrenciesDeleteResponse.status, 200 )

```



You can retrieve a list of all users who have a role defined for an application by issuing an HTTP GET request to APP_ID/roles with an app access token



```javascript

rolesResponse = client.app_id(CLIENT_ID).roles.get({access_token:APP_ACCESS_TOKEN})

```

```javascript

assert.equal( rolesResponse.status, 200 )

```



You can delete all the scores for your app by issuing an HTTP DELETE request to /APP_ID/scores with the app access_token for that app. This will reset the scores for your app. Use this if you'd like to periodically reset your game's scores and leaderboard



```javascript

scoresDeleteResponse = client.app_id(CLIENT_ID).scores.delete(null,{query:{access_token: APP_ACCESS_TOKEN}})

```

```javascript

assert.equal( scoresDeleteResponse.status, 200 )

```



You can set up a subscription by issuing an HTTP POST request to APP_ID/subscriptions with an application page access toke



```javascript

subscriptionsResponse = client.app_id(CLIENT_ID).subscriptions.get({access_token:APP_ACCESS_TOKEN})

```

```javascript

assert.equal( subscriptionsResponse.status, 200 )

```



You can set up a subscription by issuing an HTTP POST request to APP_ID/subscriptions with an application page access toke



```javascript

//not supported

// subscriptionsCreateResponse = client.app_id(CLIENT_ID).subscriptions.post({

//   "access_token" : APP_ACCESS_TOKEN,

//   "verify_token" : "API_NOTEBOOK_APP_VERIFY_TOKEN",

//   "object": "page",

//   "fields": "picture",

//   "callback_url": "http://www.my.url"

// }, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

//assert.equal( subscriptionsCreateResponse.status, 200 )

```



You can delete subscriptions by issuing an HTTP DELETE request to APP_ID/subscriptions with an application access token



```javascript

subscriptionsDeleteResponse = client.app_id(CLIENT_ID).subscriptions.delete(null, {query:{access_token:APP_ACCESS_TOKEN}})

```

```javascript

assert.equal( subscriptionsDeleteResponse.status, 200 )

```



You can upload application strings for translation by issuing an HTTP POST request to APP_ID/translations with an application access token



```javascript

translationsCreateResponse = client.app_id(CLIENT_ID).translations.post({

  "access_token" : APP_ACCESS_TOKEN,

  "canvas_url": "canvas_url=Test+about+text",

  "native_strings" : "[{\"text\":\"Test String\", \"description\": \"This is a test string for an app.\"}]"

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( translationsCreateResponse.status, 200 )

```



You can delete a translation string by issuing an HTTP DELETE request to APP_ID/translations with an application access token



```javascript

translationsDeleteResponse = client.app_id(CLIENT_ID).translations.delete(null, {query:{

  "access_token" : APP_ACCESS_TOKEN,

  "native_hashes": "[\"hash1\", \"hash2\"]"

}})

```

```javascript

assert.equal( translationsDeleteResponse.status, 200 )

```



Facebook Insights is a product available to all Pages and Apps on Facebook, and any domains claimed by a site developer using the Insights dashboard. This object represents a single Insights metric that is tied to another particular Graph API object (Page, Post, etc.).



```javascript

insightsResponse = client.app_id(CLIENT_ID).insights.get({access_token:APP_ACCESS_TOKEN})

```

```javascript

assert.equal( insightsResponse.status, 200 )

```