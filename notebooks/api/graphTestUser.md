---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/portal/pages/6772/preview
apiNotebookVersion: 1.1.66
title: Graph. Test user
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



Retrieve a list of test users



```javascript

testUsersResponse = client("/{app_id}/accounts/test-users",{app_id:CLIENT_ID}).get({"access_token": APP_ACCESS_TOKEN})

```

```javascript

assert.equal( testUsersResponse.status, 200 )

```



Create a testUser



```javascript

testUser1Response = client("/{app_id}/accounts/test-users",{app_id:CLIENT_ID}).post({

  "access_token": APP_ACCESS_TOKEN,

  "password" : "!@#RAML#@!",

  "name" : "John Smith test"

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( testUser1Response.status, 200 )

```



Create one more test user



```javascript

testUser2Response = client("/{app_id}/accounts/test-users",{app_id:CLIENT_ID}).post({

  "access_token": APP_ACCESS_TOKEN,

  "password" : "!@#RAML#@!",

  "name" : "Jach Jackson test"

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( testUser2Response.status, 200 )

```



Store IDs and access tokens of the created users



```javascript

testUserId1 = testUser1Response.body.id

testUserId2 = testUser2Response.body.id

accessToken1 = testUser1Response.body.access_token

accessToken2 = testUser2Response.body.access_token

```



You can change a test user's password by simply issuing a POST request with a password parameter to the test user ID in the Graph API.

Parameters password and name: both optional parameters. This request can be used to change the name and/or password of an existing test user.



```javascript

testUserUpdateResponse = client.TEST_USER_ID( testUserId1 ).post({"access_token": APP_ACCESS_TOKEN}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( testUserUpdateResponse.status, 200 )

```



Retrieve the test user



```javascript

testUserResponse=client.user_id(testUserId1).get({access_token:APP_ACCESS_TOKEN})

```

```javascript

assert.equal( testUserResponse.status, 200 )

```



You can use the API to make friends connections for a test user with other test users of that app. We provide an API for creating a friend request as well as for accepting a friend request. This enables developers to create several friend connections between test users via the API itself, without having to log-in as the test user to accept requests



```javascript

friendsResponse = client.TEST_USER_ID(testUserId1).friends.TEST_USER_2_ID(testUserId2).post(null,{query:{"access_token": accessToken1}})

```

```javascript

assert.equal( friendsResponse.status, 200 )

```



You can delete an existing test user like any other object in the graph



```javascript

testUser1DeleteResponse = client.TEST_USER_ID( testUserId1 ).delete(null,{query:{"access_token": APP_ACCESS_TOKEN}})

```

```javascript

assert.equal( testUser1DeleteResponse.status, 200 )

```



Delete the remaining test user



```javascript

testUser2DeleteResponse = client.TEST_USER_ID( testUserId2 ).delete(null,{query:{"access_token": APP_ACCESS_TOKEN}})

```

```javascript

assert.equal( testUser2DeleteResponse.status, 200 )

```