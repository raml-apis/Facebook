---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/portal/pages/6784/preview
apiNotebookVersion: 1.1.66
title: Open graph, objects part 1
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



Retrieve all article objects



```javascript

articleResponse = client.user_id("me").objects.article.get()

```

```javascript

assert.equal( articleResponse.status, 200 )

```



Create an article object



```javascript

articleCreateResponse = client.user_id("me").objects.article.post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"article\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Article\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\"}"

})

```

```javascript

assert.equal( articleCreateResponse.status, 200 )

```



Delete an article object



```javascript

client.object_id( articleCreateResponse.body.id ).delete()

```



Retrieve all book objects



```javascript

bookResponse = client.user_id("me").objects.book.get()

```

```javascript

assert.equal( bookResponse.status, 200 )

```



Create a book object



```javascript

bookCreateResponse = client.user_id("me").objects.book.post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"book\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Book\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\"}"

})

```

```javascript

assert.equal( bookCreateResponse.status, 200 )

```



Delete a book object



```javascript

client.object_id( bookCreateResponse.body.id ).delete()

```



Retrieve all books.author objects



```javascript

booksAuthorResponse = client("/{user_id}/objects/books.author", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( booksAuthorResponse.status, 200 )

```



Create a books.author object



```javascript

booksAuthorCreateResponse = client("/{user_id}/objects/books.author", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"books.author\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Neal Stephenson\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\",\"og:description\":\"Neal Stepheson is the author of some very long, very good books. \",\"books:gender\":\"Male\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( booksAuthorCreateResponse.status, 200 )

```



Delete a books.author object



```javascript

client.object_id( booksAuthorCreateResponse.body.id ).delete()

```



Retrieve all books.book objects



```javascript

booksBookResponse = client("/{user_id}/objects/books.book", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( booksBookResponse.status, 200 )

```



Create a books.book object



```javascript

booksBookCreateResponse = client("/{user_id}/objects/books.book", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"books.book\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Snow Crash\",\"og:image\":\"http:\/\/en.wikipedia.org\/wiki\/File:Snowcrash.jpg\",\"og:description\":\"In reality, Hiro Protagonist delivers pizza for Uncle Enzo’s CosoNostra Pizza Inc., but in the Metaverse he’s a warrior prince. Plunging headlong into the enigma of a new computer virus that’s striking down hackers everywhere, he races along the neon-lit streets on a search-and-destroy mission for the shadowy virtual villain threatening to bring about infocalypse. Snow Crash is a mind-altering romp through a future America so bizarre, so outrageous…you’ll recognize it immediately.\",\"books:isbn\":\"0553380958\",\"books:author\":\"http:\/\/samples.ogp.me\/344562145628374\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( booksBookCreateResponse.status, 200 )

```



Delete a books.book object



```javascript

client.object_id( booksBookCreateResponse.body.id ).delete()

```



Retrieve all books.genre objects



```javascript

booksGenreResponse = client("/{user_id}/objects/books.genre", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( booksGenreResponse.status, 200 )

```



Create a books.genre object



```javascript

booksGenreCreateResponse = client("/{user_id}/objects/books.genre", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"books.genre\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Genre\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\",\"books:canonical_name\":\"Adventure\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( booksGenreCreateResponse.status, 200 )

```



Delete a books.genre object



```javascript

client.object_id( booksGenreCreateResponse.body.id ).delete()

```



Retrieve all business.business objects



```javascript

businessBusinessResponse = client("/{user_id}/objects/business.business", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( businessBusinessResponse.status, 200 )

```



Create a business.business object



```javascript

businessBusinessCreateResponse = client("/{user_id}/objects/business.business", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"business.business\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Business\",\"og:image\":\"https:\/\/fbstatic-a.akamaihd.net\/images\/devsite\/attachment_blank.png\",\"business:contact_data:street_address\":\"Sample Contact data: Street Address\",\"business:contact_data:locality\":\"Sample Contact data: Locality\",\"business:contact_data:postal_code\":\"Sample Contact data: Postal Code\",\"business:contact_data:country_name\":\"Sample Contact data: Country Name\",\"place:location:latitude\":\"10\",\"place:location:longitude\":\"10\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( businessBusinessCreateResponse.status, 200 )

```



Delete a business.business object



```javascript

client.object_id( businessBusinessCreateResponse.body.id ).delete()

```



Retrieve all fitness.course objects



```javascript

fitnessCourseResponse = client("/{user_id}/objects/fitness.course", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( fitnessCourseResponse.status, 200 )

```



Create a fitness.course object



```javascript

fitnessCourseCreateResponse = client("/{user_id}/objects/fitness.course", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"fitness.course\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Course\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( fitnessCourseCreateResponse.status, 200 )

```



Delete a fitness.course object



```javascript

client.object_id( fitnessCourseCreateResponse.body.id ).delete()

```



Retrieve all fitness.unit objects



```javascript

fitnessUnitResponse = client("/{user_id}/objects/fitness.unit", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( fitnessUnitResponse.status, 200 )

```



Create a fitness.unit object



```javascript

fitnessUnitCreateResponse = client("/{user_id}/objects/fitness.unit", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"fitness.unit\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Unit\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( fitnessUnitCreateResponse.status, 200 )

```



Delete a fitness.unit object



```javascript

client.object_id( fitnessUnitCreateResponse.body.id ).delete()

```



Retrieve all game objects



```javascript

gameResponse = client.user_id("me").objects.game.get()

```

```javascript

assert.equal( gameResponse.status, 200 )

```



Create a game object



```javascript

gameCreateResponse = client.user_id("me").objects.game.post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"game\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Game\",\"og:image\":\"http:\/\/static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\"}"

})

```

```javascript

assert.equal( gameCreateResponse.status, 200 )

```



Delete a game object



```javascript

client.object_id( gameCreateResponse.body.id ).delete()

```



Retrieve all game.achievement objects



```javascript

gameAchievementResponse = client("/{user_id}/objects/game.achievement", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( gameAchievementResponse.status, 200 )

```



Create a game.achievement object



```javascript

gameAchievementCreateResponse = client("/{user_id}/objects/game.achievement", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"game.achievement\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Game Achievement\",\"og:image\":\"https:\/\/s-static.ak.fbcdn.net\/images\/devsite\/attachment_blank.png\",\"game:points\":\"10\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( gameAchievementCreateResponse.status, 200 )

```



Delete a game.achievement object



```javascript

client.object_id( gameAchievementCreateResponse.body.id ).delete()

```



Retrieve all game.stat_type objects



```javascript

gameStatTypeResponse = client("/{user_id}/objects/game.stat_type", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( gameStatTypeResponse.status, 200 )

```



Create a game.stat_type object



```javascript

gameStatTypeCreateResponse = client("/{user_id}/objects/game.stat_type", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "object": "{\"fb:app_id\":" + CLIENT_ID + ",\"og:type\":\"game.stat_type\",\"og:url\":\"http://www.my.sample.url.com\",\"og:title\":\"Sample Stat\",\"og:image\":\"https:\/\/fbstatic-a.akamaihd.net\/images\/devsite\/attachment_blank.png\",\"game:stat_number_format\":\"Sample Stat Number Format\",\"game:display_format_type\":\"Sample Display Format Type\",\"game:is_bigger_better\":\"true\"}"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( gameStatTypeCreateResponse.status, 200 )

```



Delete a game.stat_type object



```javascript

client.object_id( gameStatTypeCreateResponse.body.id ).delete()

```