---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/portal/pages/6780/preview
apiNotebookVersion: 1.1.66
title: Graph. User part 5, albums, photo, comment
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



Picture used in tests



```javascript

PICTURE_URL = "https://api-notebook.anypoint.mulesoft.com/images/notebook-graphic.png"

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

  clientId: CLIENT_ID,

  clientSecret: CLIENT_SECRET,

  scopes: [

    "publish_actions"

  ]

});

```

```javascript

ACCESS_TOKEN = $6.accessToken//access the 'accessToken' field of the response of the 4th cell which is the result of API.authenticate()

```



Retrieve the app access token



```javascript

appTokenResponse = client("/oauth/access_token").get({

  client_id : CLIENT_ID,

  client_secret : CLIENT_SECRET,

  grant_type : "client_credentials",

  scopes: [

    "publish_actions"

  ]

})

```

```javascript

APP_ACCESS_TOKEN = appTokenResponse.body.substring(appTokenResponse.body.indexOf("=")+1);

```



Achievements for the user.



```javascript

achievementsResponse = client.user_id("me").achievements.get();

```



See https://developers.facebook.com/docs/games/achievements -- defining Achievement Types



```javascript

achievementCreateResponse = client.user_id(CLIENT_ID).achievements.post({

  "access_token" : APP_ACCESS_TOKEN,

  "achievement": ACHIEVEMENT_URL

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```



Register an achievement for the user.



```javascript

achievementRegisterResponse = client.user_id("me").achievements.post({

  "access_token" : ACCESS_TOKEN,

  "achievement": ACHIEVEMENT_URL

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( achievementRegisterResponse.status, 200 )

userAchievementId = achievementRegisterResponse.body.id

```



Represents a user gaining a game achievement in a Facebook App.



```javascript

userAchievementResponse = client.achievement_id(userAchievementId).get()

```

```javascript

assert.equal( userAchievementResponse.status, 200 )

```

Unregister the achievement

```javascript

achievementsDeleteResponse = client.app_id(CLIENT_ID).achievements.post({

  "method" : "delete",

  "access_token" : APP_ACCESS_TOKEN,

  "achievement": ACHIEVEMENT_URL

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}})

```

```javascript
assert.equal( achievementsDeleteResponse.status, 200 )
```



Reading this endpoint returns an array of Album objects with the same fields as that node



```javascript

albumsResponse = client.user_id("me").albums.get()

```

```javascript

assert.equal( albumsResponse.status, 200 )

```



Create a new album



```javascript

albumCreateResponse = client.user_id("me").albums.post({

  "access_token" : ACCESS_TOKEN,

  "name" : "Notebook Test Album",

  "message" : "Running the 'facebook' API Notebook",

  "privacy" : {

    value : "EVERYONE"

  }

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```

```javascript

assert.equal( albumCreateResponse.status, 200 )

albumId = albumCreateResponse.body.id;

```



Retrieve an album



```javascript

albumResponse = client.album_id(albumId).get();

```

```javascript

assert.equal(albumResponse.status, 200);

```



You can add photos to an album by issuing an HTTP POST request



```javascript

photoCreateResponse = client.album_id(albumId).photos.post({

  "access_token" : ACCESS_TOKEN,

  "url" : PICTURE_URL

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```

```javascript

assert.equal(photoCreateResponse.status, 200);

photoId = photoCreateResponse.body.id

```



The photos contained in this album



```javascript

photosResponse = client.album_id(albumId).photos.get();

```

```javascript

assert.equal(photosResponse.status, 200);

```



Retrieve a photo by ID



```javascript

photoResponse = client.photo_id(photoId).get()

```

```javascript

assert.equal(photoResponse.status, 200);

```



Get the list of people tagged in a photo



```javascript

tagsResponse = client.photo_id(photoId).tags.get()

```

```javascript

assert.equal( tagsResponse.status, 200 )

```



You can add tags to a photo using this edge



```javascript

tagsCreateResponse = client.photo_id(photoId).tags.post({

  "access_token" : ACCESS_TOKEN,

  "tag_text" : "Testing"

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```

```javascript

assert.equal( tagsCreateResponse.status, 200 )

```



Get comment for the photo



```javascript

photoCommentsResponse = client.photo_id(photoId).comments.get()

```

```javascript

assert.equal( photoCommentsResponse.status, 200 )

```



Publish a comment



```javascript

photoCommentsCreateResponse = client.photo_id(photoId).comments.post({

  "access_token" : ACCESS_TOKEN,

  "message" : "This comment was created by the API Notebook."

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```

```javascript

assert.equal( photoCommentsCreateResponse.status, 200 )

```



Get likes for the photo



```javascript

photoLikesResponse = client.photo_id(photoId).likes.get()

```

```javascript

assert.equal( photoLikesResponse.status, 200 )

```



You can add new like



```javascript

photoLikesCreateResponse = client.photo_id(photoId).likes.post()

```

```javascript

assert.equal( photoLikesCreateResponse.status, 200 )

```



You can delete likes using this edge



```javascript

photoLikeDeleteResponse = client.photo_id(photoId).likes.delete()

```

```javascript

assert.equal( photoLikeDeleteResponse.status, 200 )

```



You can delete photos using this edge



```javascript

photoDeleteResponse = client.photo_id(photoId).delete()

```

```javascript

assert.equal( photoDeleteResponse.status, 200 )

```



Create a comment on the album.



```javascript

albumCommentCreateResponse = client.album_id(albumId).comments.post({

  "access_token" : ACCESS_TOKEN,

  "message": "This comment was created by the API Notebook"

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```

```javascript

assert.equal(albumCommentCreateResponse.status, 200);

commentId = albumCommentCreateResponse.body.id;

```



Retrieve album comments



```javascript

albumCommentsResponse = client.album_id(albumId).comments.get();

```

```javascript

assert.equal(albumCommentsResponse.status, 200);

```



You can like an Album by issuing an HTTP POST request



```javascript

albumLikeCreateResponse = client.album_id(albumId).likes.post({headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```

```javascript

assert.equal(albumLikeCreateResponse.status, 200);

```



The likes made on this album



```javascript

albumLikesResponse = client.album_id(albumId).likes.get();

```

```javascript

assert.equal(albumLikesResponse.status, 200);

```



You can unlike an album by issuing an HTTP DELETE request



```javascript

albumLikesResponse = client.album_id(albumId).likes.delete();

```

```javascript

assert.equal(albumLikesResponse.status, 200);

```



The album's cover photo, the first picture uploaded to an album becomes the cover photo for the album



```javascript

pictureResponse = client.album_id(albumId).picture.get();

```

```javascript

assert.equal(pictureResponse.status, 200);

```



Retrieve a single comment



```javascript

commentResponse = client.comment_id(commentId).get();

```

```javascript

assert.equal(commentResponse.status, 200);

```

You can delete an achievement for a player.

```javascript

deleteAchievementsResponse = client.user_id("me").achievements.delete(null,{query:{

  "achievement": ACHIEVEMENT_URL

}})

```

```javascript
assert.equal( deleteAchievementsResponse.status, 200 )
```



A user access token with publish_actions permission is required to delete a comment posted by that user



```javascript

commentDeleteResponse = client.comment_id(commentId).delete();

```

```javascript

assert.equal(commentDeleteResponse.status, 200);

```