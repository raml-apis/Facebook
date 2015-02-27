---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/portal/pages/6781/preview
apiNotebookVersion: 1.1.66
title: Graph. Friendlist, group part 2, link, group doc
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

```javascript

// Read about the Facebook RAML API at https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/contracts

API.createClient('client', '/apiplatform/repository/public/organizations/30/apis/7965/versions/8129/definition');

```

```javascript

API.authenticate(client,"oauth_2_0",{

  clientId: CLIENT_ID,

  clientSecret: CLIENT_SECRET,

  scopes: [

    "read_friendlists", "manage_friendlists"

  ]

});

```

```javascript

ACCESS_TOKEN = $4.accessToken//access the 'accessToken' field of the response of the 4th cell which is the result of API.authenticate()

```

```javascript

IS_ADMIN = window.confirm("Some methods in this notebook require you to be an admin, a developer or a tester of your Facebook application. Press 'OK' if you have one of these roles.\n\nOtherwise press 'CANCEL'. In this case Notebook will not try executing these methods.")

```



Retrieve app access token



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



Get friendlists



```javascript

friendlistsResponse = client.user_id("me").friendlists.get();

```



PIck some friendlist ID



```javascript

friendListId = friendlistsResponse.body.data[0].id;

```



Retrieve a particular friendlist.



```javascript

friendListResponse = client.friendlist_id(friendListId).get();

```

```javascript

assert.equal(friendListResponse.status, 200);

```



Permissions

A user access token with read_friendlists permission is required to view the members of any of the current person's friend lists.



You can add people to a friend list using this edge, while the /{user_id}/friendlists edge will let you create new friend lists



```javascript

// not supported

// membersResponse = client.friendlist_id(friendListId).members.post({

//   "access_token": ACCESS_TOKEN,

//   "members": 1

// },{headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```

```javascript

//assert.equal(membersResponse.status, 200);

```



Retrieve all friendlist members.



```javascript

membersResponse = client.friendlist_id(friendListId).members.get();

```

```javascript

assert.equal(membersResponse.status, 200);

```



You can remove people from a list by using this edge, while you can delete lists themselves using the /{friendlist_id} node



manage_friendlists already in the scope but "(#297) Requires extended permission: manage_friendlists" still shows



```javascript

// not supported

// membersResponse = client.friendlist_id(friendListId).members.delete(null,{query:{

//   "members": friendId

// }});

```

```javascript

//assert.equal(membersResponse.status, 200);

```



Create a group



```javascript

groupsCreateResponse = client.app_id(CLIENT_ID).groups.post({

  "access_token" : APP_ACCESS_TOKEN,

  "name": "API Notebook Test Group",

  "privacy": "open"

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```

```javascript

assert.equal(groupsCreateResponse.status, 200);

groupId = groupsCreateResponse.body.id

```



Retrieve the group



```javascript

groupResponse = client.group_id(groupId).get();

```

```javascript

assert.equal(groupResponse.status, 200);

```



A list of Group docs



```javascript

docsResponse = client.group_id(groupId).docs.get();

```

```javascript

assert.equal(docsResponse.status,200)

```

```javascript

if(IS_ADMIN){

  testDoc = window.confirm("If you whish to test retreiving a single doc, you should create one in the newly created group before you continue executing the notebook. After you press 'ok' you'll be added into the list of group members. Then you'll be given a pause so that you could create a document.")

}

```

```javascript

if(IS_ADMIN && testDoc){

  userAddResponse = client.group_id(groupId).members.USER_ID(client.user_id("me").get().body.id).post({

    "access_token": APP_ACCESS_TOKEN

  },{headers: {"Content-Type":"application/x-www-form-urlencoded"}});

}

```

```javascript

if(IS_ADMIN && testDoc){

  window.alert("You have become a group member. Please, visit the group file storage an create a document inside the group. The URL is\n\nhttps://www.facebook.com/groups/"+groupId+"/files/")

}

```

```javascript

if(IS_ADMIN && testDoc){

  docsResponse2 = client.group_id(groupId).docs.get();

  docResponse = client.group_doc_id(docsResponse2.body.data[0].id).get()

}

```

```javascript

if(IS_ADMIN && testDoc){

  assert.equal(docResponse.status,200)

}

```



The files uploaded to this group.



```javascript

filesResponse = client.group_id(groupId).files.get();

```

```javascript

assert.equal(filesResponse.status,200)

```



Update that group's cover photo.



```javascript

groupIdResponse = client.group_id(groupId).post({

  "access_token": APP_ACCESS_TOKEN,

  "cover_url": "https://api-notebook.anypoint.mulesoft.com/images/notebook-graphic.png"

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```

```javascript

assert.equal(groupIdResponse.status, 200);

```



A list of group's events.



```javascript

eventsResponse = client.group_id(groupId).events.get();

```

```javascript

assert.equal(eventsResponse.status, 200);

```



The feed of posts (including status updates) and links published to this group.



```javascript

feedResponse = client.group_id(groupId).feed.get();

```

```javascript

assert.equal(feedResponse.status, 200);

```



Post a message on the group's wall.



```javascript

feedResponse = client.group_id(groupId).feed.post({

  "access_token": APP_ACCESS_TOKEN,

  "message":"This message was created by the 'facebook' API Notebook"

},{headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```

```javascript

assert.equal(feedResponse.status, 200);

```



Share a link on the wall.



```javascript

linksResponse = client.user_id("me").feed.post({

  "access_token": ACCESS_TOKEN,

  "link": "https://pp.vk.me/c540107/c540103/v540103852/2e165/yg6M6MSyEPU.jpg"

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}});

linksId = linksResponse.body.id;

```



Retrieve a link.



```javascript

linkResponse = client.link_id(linksId).get();

```

```javascript

assert.equal(linkResponse.status, 200);

```



An app can delete a link if it published

Permissions

A user access token with publish_actions permission.



```javascript

linkDeleteResponse = client.link_id(linksId).delete();

```

```javascript

assert.equal(linkDeleteResponse.status, 200);

```



Delete group members.



```javascript

groupMembers = client.group_id(groupId).members.get().body.data

for(var ind in groupMembers){

  var memberId = groupMembers[ind].id

  client.group_id(groupId).members.USER_ID(memberId).delete({

    "access_token": APP_ACCESS_TOKEN

  },{headers: {"Content-Type":"application/x-www-form-urlencoded"}});

}

```



Delete the group



```javascript

client.app_id(CLIENT_ID).groups.GROUP_ID(groupId).delete({

  "access_token" : APP_ACCESS_TOKEN

})

```