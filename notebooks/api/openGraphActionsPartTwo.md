---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/portal/pages/6783/preview
apiNotebookVersion: 1.1.66
title: Open graph, actions part 2
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

  clientId: CLIENT_ID,

  clientSecret: CLIENT_SECRET,

  scopes: [ "publish_actions" ]

})

```

```javascript

ACCESS_TOKEN = $4.accessToken//access the 'accessToken' field of the response of the API.authenticate() cell

```

```javascript

IS_ADMIN = window.confirm("Some methods in this notebook require you to be an admin, a developer or a tester of your Facebook application. Press 'OK' if you have one of these roles.\n\nOtherwise press 'CANCEL'. In this case Notebook will not try executing these methods.")

```



Retrieve all music.listens objects



```javascript

musicListensResponse = client("/{user_id}/music.listens", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( musicListensResponse.status, 200 )

```



Create a music.listens object



```javascript

//This method is not allowed for all countries. This rtestriction does not affect admins testers and developers of facebook application used.

musicListensCreateResponse = client("/{user_id}/music.listens", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "album": "http://samples.ogp.me/461258347226565",

  "musician": "http://samples.ogp.me/390580850990722",

  "playlist": "http://samples.ogp.me/461258467226553",

  "radio_station": "http://samples.ogp.me/461258533893213",

  "song": "http://samples.ogp.me/461258627226537"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert( musicListensCreateResponse.status == 200 || musicListensCreateResponse.status == 403 )

```



Delete a music.listens object



```javascript

client.object_id( musicListensCreateResponse.body.id ).delete()

```



Retrieve all music.playlists objects



```javascript

musicPlaylistsResponse = client("/{user_id}/music.playlists", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( musicPlaylistsResponse.status, 200 )

```



Delete a music.playlists object which could have been created during earlier notebook runs.



```javascript

for(var ind in musicPlaylistsResponse.body.data){

  var item = musicPlaylistsResponse.body.data[ind]

  var data = item.data

  if(data){

    var obj = data.playlist

    if(obj){

      var url = obj.url

      if(url){

        if(url=="http://samples.ogp.me/461258467226553"){

          client.object_id(item.id).delete()

        }

      }

    }

  }

}

```



Create a music.playlists object



```javascript

musicPlaylistsCreateResponse = client("/{user_id}/music.playlists", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "playlist": "http://samples.ogp.me/461258467226553"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( musicPlaylistsCreateResponse.status, 200 )

```



Delete a music.playlists object



```javascript

client.object_id( musicPlaylistsCreateResponse.body.id ).delete()

```



Retrieve all news.publishes objects



```javascript

newsPublishesResponse = client("/{user_id}/news.publishes", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( newsPublishesResponse.status, 200 )

```



Create a news.publishes object



```javascript

if(IS_ADMIN){

  newsPublishesCreateResponse = client("/{user_id}/news.publishes", {

    "user_id": "me"

  }).post({

    "access_token" : ACCESS_TOKEN,

    "article": "http://samples.ogp.me/434264856596891"

  }, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

}

```

```javascript

if(IS_ADMIN){

  assert.equal( newsPublishesCreateResponse.status, 200 )

}

```



Delete a news.publishes object



```javascript

if(IS_ADMIN){

  client.object_id( newsPublishesCreateResponse.body.id ).delete()

}

```



Retrieve all news.reads objects



```javascript

newsReadsResponse = client("/{user_id}/news.reads", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( newsReadsResponse.status, 200 )

```



Delete a news.reads object which could have been created during earlier notebook runs.



```javascript

for(var ind in newsReadsResponse.body.data){

  var item = newsReadsResponse.body.data[ind]

  var data = item.data

  if(data){

    var obj = data.article

    if(obj){

      var url = obj.url

      if(url){

        if(url=="http://samples.ogp.me/434264856596891"){

          client.object_id(item.id).delete()

        }

      }

    }

  }

}

```



Create a news.reads object



```javascript

if(IS_ADMIN){

  newsReadsCreateResponse = client("/{user_id}/news.reads", {

    "user_id": "me"

  }).post({

    "access_token" : ACCESS_TOKEN,

    "article": "http://samples.ogp.me/434264856596891"

  }, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

}

```

```javascript

if(IS_ADMIN){

  assert.equal( newsReadsCreateResponse.status, 200 )

}

```



Delete a news.reads object



```javascript

if(IS_ADMIN){

  client.object_id( newsReadsCreateResponse.body.id ).delete()

}

```



Retrieve all og.follows objects



```javascript

ogFollowsResponse = client("/{user_id}/og.follows", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( ogFollowsResponse.status, 200 )

```



Delete an og.follows object which could have been created during earlier notebook runs.



```javascript

for(var ind in ogFollowsResponse.body.data){

  var item = ogFollowsResponse.body.data[ind]

  var data = item.data

  if(data){

    var obj = data.profile

    if(obj){

      var url = obj.url

      if(url){

        if(url=="http://samples.ogp.me/390580850990722"){

          client.object_id(item.id).delete()

        }

      }

    }

  }

}

```



Create a og.follows object



```javascript

if(IS_ADMIN){

  ogFollowsCreateResponse = client("/{user_id}/og.follows", {

    "user_id": "me"

  }).post({

    "access_token" : ACCESS_TOKEN,

    "profile": "http://samples.ogp.me/390580850990722"

  }, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

}

```

```javascript

if(IS_ADMIN){

  assert.equal( ogFollowsCreateResponse.status, 200 )

}

```



Delete a og.follows object



```javascript

if(IS_ADMIN){

  client.object_id( ogFollowsCreateResponse.body.id ).delete()

}

```



Retrieve all og.likes objects



```javascript

ogLikesResponse = client("/{user_id}/og.likes", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( ogLikesResponse.status, 200 )

```



Delete an og.likes object which could have been created during earlier notebook runs



```javascript

for(var ind in ogLikesResponse.body.data){

  var item = ogLikesResponse.body.data[ind]

  var data = item.data

  if(data){

    var obj = data.object

    if(obj){

      var url = obj.url

      if(url){

        if(url=="http://samples.ogp.me/226075010839791"){

          client.object_id(item.id).delete()

        }

      }

    }

  }

}

```



Create a og.likes object



```javascript

if(IS_ADMIN){

  ogLikesCreateResponse = client("/{user_id}/og.likes", {

    "user_id": "me"

  }).post({

    "access_token" : ACCESS_TOKEN,

    "object": "http://samples.ogp.me/226075010839791"

  }, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

}

```

```javascript

if(IS_ADMIN){

  assert.equal( ogLikesCreateResponse.status, 200 )

}

```



Delete a og.likes object



```javascript

if(IS_ADMIN){

  client.object_id( ogLikesCreateResponse.body.id ).delete()

}

```



Retrieve all places.saves objects



```javascript

placesSavesResponse = client("/{user_id}/places.saves", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( placesSavesResponse.status, 200 )

```



We need a place object in order to invoke place.saves actions.



```javascript

placeCreateResponse = client.user_id("me").objects.place.post({

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"place\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Place\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\",\"place:location:latitude\":\"10\",\"place:location:longitude\":\"10\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```



Create a places.saves object



```javascript

if(IS_ADMIN){

  placesSavesCreateResponse = client("/{user_id}/places.saves", {

    "user_id": "me"

  }).post({

    "access_token" : ACCESS_TOKEN,

    "generic_place": placeCreateResponse.body.id

  }, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

}

```

```javascript

if(IS_ADMIN){

  assert.equal( placesSavesCreateResponse.status, 200 )

}

```



Delete a places.saves and a place objects



```javascript

if(IS_ADMIN){

  client.object_id( placesSavesCreateResponse.body.id ).delete()

  client.object_id( placeCreateResponse.body.id ).delete()

}

```



Retrieve all video.rates objects



```javascript

videoRatesResponse = client("/{user_id}/video.rates", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( videoRatesResponse.status, 200 )

```



We need a video.movie object in order to invoke video.* actions.



```javascript

videoMovieCreateResponse = client("/{user_id}/objects/video.movie", {

  "user_id": "me"

}).post({

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"video.movie\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Movie\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```



Create a video.rates object



```javascript

if(IS_ADMIN){

  videoRatesCreateResponse = client("/{user_id}/video.rates", {

    "user_id": "me"

  }).post({

    "access_token" : ACCESS_TOKEN,

    "movie": videoMovieCreateResponse.body.id

  }, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

}

```

```javascript

if(IS_ADMIN){

  assert.equal( videoRatesCreateResponse.status, 200 )

}

```



Delete a video.rates object



```javascript

if(IS_ADMIN){

  client.object_id( videoRatesCreateResponse.body.id ).delete()

}

```



Retrieve all video.wants_to_watch objects



```javascript

videoWantsToWatchResponse = client("/{user_id}/video.wants_to_watch", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( videoWantsToWatchResponse.status, 200 )

```



Create a video.wants_to_watch object



```javascript

if(IS_ADMIN){

  videoWantsToWatchCreateResponse = client("/{user_id}/video.wants_to_watch", {

    "user_id": "me"

  }).post({

    "access_token" : ACCESS_TOKEN,

    "movie": videoMovieCreateResponse.body.id

  }, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

}

```

```javascript

if(IS_ADMIN){

  assert.equal( videoWantsToWatchCreateResponse.status, 200 )

}

```



Delete a video.wants_to_watch object



```javascript

if(IS_ADMIN){

  client.object_id( videoWantsToWatchCreateResponse.body.id ).delete()

}

```



Retrieve all video.watches objects



```javascript

videoWatchesResponse = client("/{user_id}/video.watches", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( videoWatchesResponse.status, 200 )

```



Create a video.watches object



```javascript

if(IS_ADMIN){

  videoWatchesCreateResponse = client("/{user_id}/video.watches", {

    "user_id": "me"

  }).post({

    "access_token" : ACCESS_TOKEN,

    "movie": videoMovieCreateResponse.body.id

  }, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

}

```

```javascript

if(IS_ADMIN){

  assert.equal( videoWatchesCreateResponse.status, 200 )

}

```



Delete a video.watches object



```javascript

if(IS_ADMIN){

  client.object_id( videoWatchesCreateResponse.body.id ).delete()

}

```