---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/portal/pages/6785/preview
apiNotebookVersion: 1.1.66
title: Open graph, objects part 2
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

  clientSecret: CLIENT_SECRET

})

```

```javascript

ACCESS_TOKEN = $4.accessToken//access the 'accessToken' field of the response of the API.authenticate() cell

```



Retrieve all music.album objects



```javascript

musicAlbumResponse = client("/{user_id}/objects/music.album", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( musicAlbumResponse.status, 200 )

```



Create a music.album object



```javascript

musicAlbumCreateResponse = client("/{user_id}/objects/music.album", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"music.album\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Album\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( musicAlbumCreateResponse.status, 200 )

```



Delete a music.album object



```javascript

client.object_id( musicAlbumCreateResponse.body.id ).delete()

```



Retrieve all music.musician objects



```javascript

musicMusicianResponse = client("/{user_id}/objects/music.musician", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( musicMusicianResponse.status, 200 )

```



Create a music.musician object



```javascript

musicMusicianCreateResponse = client("/{user_id}/objects/music.musician", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"music.musician\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Musician\",\"og:image\":\"https:\/\/fbstatic-a.akamaihd.net\/images\/devsite\/attachment_blank.png\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( musicMusicianCreateResponse.status, 200 )

```



Delete a music.musician object



```javascript

client.object_id( musicMusicianCreateResponse.body.id ).delete()

```



Retrieve all music.playlist objects



```javascript

musicPlaylistResponse = client("/{user_id}/objects/music.playlist", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( musicPlaylistResponse.status, 200 )

```



Create a music.playlist object



```javascript

musicPlaylistCreateResponse = client("/{user_id}/objects/music.playlist", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"music.playlist\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Music Playlist\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( musicPlaylistCreateResponse.status, 200 )

```



Delete a music.playlist object



```javascript

client.object_id( musicPlaylistCreateResponse.body.id ).delete()

```



Retrieve all music.radio_station objects



```javascript

musicRadioStationResponse = client("/{user_id}/objects/music.radio_station", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( musicRadioStationResponse.status, 200 )

```



Create a music.radio_station object



```javascript

musicRadioStationCreateResponse = client("/{user_id}/objects/music.radio_station", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"music.radio_station\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Radio Station\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( musicRadioStationCreateResponse.status, 200 )

```



Delete a music.radio_station object



```javascript

client.object_id( musicRadioStationCreateResponse.body.id ).delete()

```



Retrieve all music.song objects



```javascript

musicSongResponse = client("/{user_id}/objects/music.song", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( musicSongResponse.status, 200 )

```



Create a music.song object



```javascript

musicSongCreateResponse = client("/{user_id}/objects/music.song", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"music.song\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Song\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( musicSongCreateResponse.status, 200 )

```



Delete a music.song object



```javascript

client.object_id( musicSongCreateResponse.body.id ).delete()

```



Retreive all object objects



```javascript

objectResponse = client.user_id("me").objects.object.get()

```

```javascript

assert.equal( objectResponse.status, 200 )

```



Create an object object



```javascript

objectCreateResponse = client.user_id("me").objects.object.post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"object\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Object\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\"}"

})

```

```javascript

assert.equal( objectCreateResponse.status, 200 )

```



Delete a object object



```javascript

client.object_id( objectCreateResponse.body.id ).delete()

```



Retrieve all place objects



```javascript

placeResponse = client.user_id("me").objects.place.get()

```

```javascript

assert.equal( placeResponse.status, 200 )

```



Create a place object



```javascript

placeCreateResponse = client.user_id("me").objects.place.post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"place\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Place\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\",\"place:location:latitude\":\"10\",\"place:location:longitude\":\"10\"}"

})

```

```javascript

assert.equal( placeCreateResponse.status, 200 )

```



Delete a place object



```javascript

client.object_id( placeCreateResponse.body.id ).delete()

```



Retrieve all product objects



```javascript

productResponse = client.user_id("me").objects.product.get()

```

```javascript

assert.equal( productResponse.status, 200 )

```



Create a product object



```javascript

productCreateResponse = client.user_id("me").objects.product.post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"product\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Product\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\",\"product:original_price:amount\":\"100 \",\"product:original_price:currency\":\"usd\",\"product:pretax_price:amount\":\"70\",\"product:pretax_price:currency\":\"usd \",\"product:price:amount\":\"100\",\"product:price:currency\":\"usd\",\"product:shipping_cost:amount\":\"15 \",\"product:shipping_cost:currency\":\"usd\",}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( productCreateResponse.status, 200 )

```



Delete a product object



```javascript

client.object_id( productCreateResponse.body.id ).delete()

```



Retrieve all product.group objects



```javascript

productGroupResponse = client("/{user_id}/objects/product.group", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( productGroupResponse.status, 200 )

```



Create a product.group object



```javascript

productGroupCreateResponse = client("/{user_id}/objects/product.group", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"product.group\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Group\",\"og:image\":\"https:\/\/fbstatic-a.akamaihd.net\/images\/devsite\/attachment_blank.png\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( productGroupCreateResponse.status, 200 )

```



Delete a product.group object



```javascript

client.object_id( productGroupCreateResponse.body.id ).delete()

```



Retrieve all product.item objects



```javascript

productItemResponse = client("/{user_id}/objects/product.item", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( productItemResponse.status, 200 )

```



Create a product.item object



```javascript

productItemCreateResponse = client("/{user_id}/objects/product.item", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"product.item\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Item\",\"og:image\":\"https:\/\/fbstatic-a.akamaihd.net\/images\/devsite\/attachment_blank.png\",\"product:retailer_item_id\":\"Sample Retailer Product Item ID\",\"product:price:amount\":\"100\",\"product:price:currency\":\"usd\",\"product:shipping_cost:amount\":\"15 \",\"product:shipping_cost:currency\":\"usd\",\"product:availability\":\"in stock\",\"product:condition\":\"new\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( productItemCreateResponse.status, 200 )

```



Delete a product.item object



```javascript

client.object_id( productItemCreateResponse.body.id ).delete()

```



Retrieve all profile objects



```javascript

profileResponse = client.user_id("me").objects.profile.get()

```

```javascript

assert.equal( profileResponse.status, 200 )

```



Create a profile object



```javascript

profileCreateResponse = client.user_id("me").objects.profile.post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"profile\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Profile\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( profileCreateResponse.status, 200 )

```



Delete a profile object



```javascript

client.object_id( profileCreateResponse.body.id ).delete()

```