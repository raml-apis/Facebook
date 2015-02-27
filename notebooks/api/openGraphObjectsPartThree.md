---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/portal/pages/6786/preview
apiNotebookVersion: 1.1.66
title: Open graph, objects part 3
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



Retrieve all quick_election.election objects



```javascript

quickElectionElectionResponse = client("/{user_id}/objects/quick_election.election", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( quickElectionElectionResponse.status, 200 )

```



Create a quick_election.election object



```javascript

quickElectionElectionCreateResponse = client("/{user_id}/objects/quick_election.election", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"quick_election.election\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Election\",\"og:image\":\"https:\/\/fbstatic-a.akamaihd.net\/images\/devsite\/attachment_blank.png\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( quickElectionElectionCreateResponse.status, 200 )

```



Delete a quick_election.election object



```javascript

client.object_id( quickElectionElectionCreateResponse.body.id ).delete()

```



Retrieve all restaurant.menu objects



```javascript

restaurantMenuResponse = client("/{user_id}/objects/restaurant.menu", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( restaurantMenuResponse.status, 200 )

```



Create a restaurant.menu object



```javascript

restaurantMenuCreateResponse = client("/{user_id}/objects/restaurant.menu", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"restaurant.menu\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Menu\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\",\"restaurant:restaurant\":\"http://www.my.restaurant.org\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( restaurantMenuCreateResponse.status, 200 )

```



Delete a restaurant.menu object



```javascript

client.object_id( restaurantMenuCreateResponse.body.id ).delete()

```



Retrieve all restaurant.menu_item objects



```javascript

restaurantMenuItemResponse = client("/{user_id}/objects/restaurant.menu_item", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( restaurantMenuItemResponse.status, 200 )

```



Create a restaurant.menu_item object



```javascript

restaurantMenuItemCreateResponse = client("/{user_id}/objects/restaurant.menu_item", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"restaurant.menu_item\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Menu Item\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\",\"restaurant:section\":\"http://www.my.restaurant.section.org\",\"restaurant:variation:price:amount\":\"15\",\"restaurant:variation:price:currency\":\"usd\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( restaurantMenuItemCreateResponse.status, 200 )

```



Delete a restaurant.menu_item object



```javascript

client.object_id( restaurantMenuItemCreateResponse.body.id ).delete()

```



Retrieve all restaurant.menu_section objects



```javascript

restaurantMenuSectionResponse = client("/{user_id}/objects/restaurant.menu_section", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( restaurantMenuSectionResponse.status, 200 )

```



Create a restaurant.menu_section object



```javascript

restaurantMenuSectionCreateResponse = client("/{user_id}/objects/restaurant.menu_section", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"restaurant.menu_section\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Menu Section\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\",\"restaurant:menu\":\"http://www.my.meny.com\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( restaurantMenuSectionCreateResponse.status, 200 )

```



Delete a restaurant.menu_section object



```javascript

client.object_id( restaurantMenuSectionCreateResponse.body.id ).delete()

```



Retrieve all restaurant.restaurant objects



```javascript

restaurantRestaurantResponse = client("/{user_id}/objects/restaurant.restaurant", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( restaurantRestaurantResponse.status, 200 )

```



Create a restaurant.restaurant object



```javascript

restaurantRestaurantCreateResponse = client("/{user_id}/objects/restaurant.restaurant", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"restaurant.restaurant\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Restaurant\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\",\"og:description\":\"This place rocks\",\"restaurant:contact_info:street_address\":\"1601 Willow Rd.\",\"restaurant:contact_info:locality\":\"Menlo Park\",\"restaurant:contact_info:region\":\"California\",\"restaurant:contact_info:postal_code\":\"94025\",\"restaurant:contact_info:country_name\":\"United States\",\"restaurant:contact_info:email\":\"brian@example.com\",\"restaurant:contact_info:phone_number\":\"212-555-1234\",\"restaurant:contact_info:website\":\"http:\/\/www.facebook.com\",\"place:location:latitude\":\"37.484828\",\"place:location:longitude\":\"-122.148283\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( restaurantRestaurantCreateResponse.status, 200 )

```



Delete a restaurant.restaurant object



```javascript

client.object_id( restaurantRestaurantCreateResponse.body.id ).delete()

```



Retrieve all video.episode objects



```javascript

videoEpisodeResponse = client("/{user_id}/objects/video.episode", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( videoEpisodeResponse.status, 200 )

```



Create a video.episode object



```javascript

videoEpisodeCreateResponse = client("/{user_id}/objects/video.episode", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"video.episode\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample TV Episode\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( videoEpisodeCreateResponse.status, 200 )

```



Delete a video.episode object



```javascript

client.object_id( videoEpisodeCreateResponse.body.id ).delete()

```



Retrieve all video.movie objects



```javascript

videoMovieResponse = client("/{user_id}/objects/video.movie", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( videoMovieResponse.status, 200 )

```



Create a video.movie object



```javascript

videoMovieCreateResponse = client("/{user_id}/objects/video.movie", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"video.movie\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Movie\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( videoMovieCreateResponse.status, 200 )

```



Delete a video.movie object



```javascript

client.object_id( videoMovieCreateResponse.body.id ).delete()

```



Retrieve all video.other objects



```javascript

videoOtherResponse = client("/{user_id}/objects/video.other", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( videoOtherResponse.status, 200 )

```



Create a video.other object



```javascript

videoOtherCreateResponse = client("/{user_id}/objects/video.other", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"video.other\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Video\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( videoOtherCreateResponse.status, 200 )

```



Delete a video.other object



```javascript

client.object_id( videoOtherCreateResponse.body.id ).delete()

```



Retrieve all video.tv_show objects



```javascript

videoTvShowResponse = client("/{user_id}/objects/video.tv_show", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( videoTvShowResponse.status, 200 )

```



Create a video.tv_show object



```javascript

videoTvShowCreateResponse = client("/{user_id}/objects/video.tv_show", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"video.tv_show\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample TV Show\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( videoTvShowCreateResponse.status, 200 )

```



Delete a video.tv_show object



```javascript

client.object_id( videoTvShowCreateResponse.body.id ).delete()

```



Retrieve all website objects



```javascript

websiteResponse = client.user_id("me").objects.website.get()

```

```javascript

assert.equal( websiteResponse.status, 200 )

```



Create a website object



```javascript

websiteCreateResponse = client.user_id("me").objects.website.post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"website\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Website\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\"}"

})

```

```javascript

assert.equal( websiteCreateResponse.status, 200 )

```



Delete a website object



```javascript

websiteDeleteresponse = client.object_id( websiteCreateResponse.body.id ).delete()

```

```javascript

assert.equal(websiteDeleteresponse.status,200)

```