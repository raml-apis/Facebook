---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/portal/pages/6782/preview
apiNotebookVersion: 1.1.66
title: Open graph, actions part 1
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

  scopes: [ "publish_actions" ]

})

```

```javascript

ACCESS_TOKEN = $5.accessToken//access the 'accessToken' field of the response of the API.authenticate() cell

```



Retrieving the application access token.



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



Retrieve all books.quotes objects



```javascript

booksQuotesResponse = client("/{user_id}/books.quotes", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( booksQuotesResponse.status, 200 )

```



Create a books.quotes object



```javascript

booksQuotesCreateResponse = client("/{user_id}/books.quotes", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "body": "Some Arbitrary String",

  "book": "http://samples.ogp.me/344468272304428"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( booksQuotesCreateResponse.status, 200 )

```



Delete a books.quotes object



```javascript

client.object_id( booksQuotesCreateResponse.body.id ).delete()

```



Retrieve all books.rates objects



```javascript

booksRatesResponse = client("/{user_id}/books.rates", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( booksRatesResponse.status, 200 )

```



Create a books.rates object



```javascript

booksRatesCreateResponse = client("/{user_id}/books.rates", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "book": "http://samples.ogp.me/344468272304428",

  "rating[value]" : "3.1415926535",

  "rating[scale]": "42"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( booksRatesCreateResponse.status, 200 )

```



Delete a books.rates object



```javascript

client.object_id( booksRatesCreateResponse.body.id ).delete()

```



Retrieve all books.reads objects



```javascript

booksReadsResponse = client("/{user_id}/books.reads", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( booksReadsResponse.status, 200 )

```



Create a books.reads object



```javascript

booksReadsCreateResponse = client("/{user_id}/books.reads", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "book": "http://samples.ogp.me/344468272304428",

  "progress" : {

    "timestamp": "1407351164",

    "percent_complete": "3.1415926535"

  }

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( booksReadsCreateResponse.status, 200 )

```



Delete a books.reads object



```javascript

client.object_id( booksReadsCreateResponse.body.id ).delete()

```



Retrieve all books.wants_to_read objects



```javascript

booksWantsToReadResponse = client("/{user_id}/books.wants_to_read", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( booksWantsToReadResponse.status, 200 )

```



Create a books.wants_to_read object



```javascript

booksWantsToReadCreateResponse = client("/{user_id}/books.wants_to_read", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "book": "http://samples.ogp.me/344468272304428"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( booksWantsToReadCreateResponse.status, 200 )

```



Delete a books.wants_to_read object



```javascript

client.object_id( booksWantsToReadCreateResponse.body.id ).delete()

```



Retrieve all fitness.bikes objects



```javascript

fitnessBikesResponse = client("/{user_id}/fitness.bikes", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( fitnessBikesResponse.status, 200 )

```



Create a fitness.bikes object



```javascript

fitnessBikesCreateResponse = client("/{user_id}/fitness.bikes", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "course": "http://samples.ogp.me/136756249803614"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( fitnessBikesCreateResponse.status, 200 )

```



Delete a fitness.bikes object



```javascript

client.object_id( fitnessBikesCreateResponse.body.id ).delete()

```



Retrieve all fitness.runs objects



```javascript

fitnessRunsResponse = client("/{user_id}/fitness.runs", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( fitnessRunsResponse.status, 200 )

```



Create a fitness.runs object



```javascript

fitnessRunsCreateResponse = client("/{user_id}/fitness.runs", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "course": "http://samples.ogp.me/136756249803614"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( fitnessRunsCreateResponse.status, 200 )

```



Delete a fitness.runs object



```javascript

client.object_id( fitnessRunsCreateResponse.body.id ).delete()

```



Retrieve all fitness.walks objects



```javascript

fitnessWalksResponse = client("/{user_id}/fitness.walks", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( fitnessWalksResponse.status, 200 )

```



Create a fitness.walks object



```javascript

fitnessWalksCreateResponse = client("/{user_id}/fitness.walks", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "course": "http://samples.ogp.me/136756249803614"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( fitnessWalksCreateResponse.status, 200 )

```



Delete Create a fitness.walks object



```javascript

client.object_id( fitnessWalksCreateResponse.body.id ).delete()

```



Retrieve all games.achieves objects



```javascript

gamesAchievesResponse = client("/{user_id}/games.achieves", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( gamesAchievesResponse.status, 200 )

```



Register an application achievement for the application.



```javascript

achievementCreateResponse = client.user_id(CLIENT_ID).achievements.post({

  "access_token" : APP_ACCESS_TOKEN,

  "achievement": ACHIEVEMENT_URL

}, {headers: {"Content-Type":"application/x-www-form-urlencoded"}});

```



Delete a games.achieves object which could have been created during earlier notebook runs.



```javascript

for(var ind in gamesAchievesResponse.body.data){

  var item = gamesAchievesResponse.body.data[ind]

  var data = item.data

  if(data){

    var obj = data.achievement

    if(obj){

      var url = obj.url

      if(url){

        if(url==ACHIEVEMENT_URL){

          client.object_id(item.id).delete()

        }

      }

    }

  }

}

```



Now we can register achievement for the user

Create a games.achieves object





```javascript

gamesAchievesCreateResponse = client("/{user_id}/games.achieves", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "achievement": ACHIEVEMENT_URL

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( gamesAchievesCreateResponse.status, 200 )

```



Delete a games.achieves object



```javascript

client.object_id( gamesAchievesCreateResponse.body.id ).delete()

```



Retrieve all games.celebrate objects



```javascript

gamesCelebrateResponse = client("/{user_id}/games.celebrate", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( gamesCelebrateResponse.status, 200 )

```



Create a games.celebrate object



```javascript

gamesCelebrateCreateResponse = client("/{user_id}/games.celebrate", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "victory": "http://samples.ogp.me/500426766634310"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( gamesCelebrateCreateResponse.status, 200 )

```



Delete a games.celebrate object



```javascript

client.object_id( gamesCelebrateCreateResponse.body.id ).delete()

```



Retrieve all games.passes objects



```javascript

gamesPassesResponse = client("/{user_id}/games.passes", {

  "user_id": "me"

}).get()

```

```javascript

assert.equal( gamesPassesResponse.status, 200 )

```



Create a games.passes object



```javascript

gamesPassesCreateResponse = client("/{user_id}/games.passes", {

  "user_id": "me"

}).post({

  "access_token" : ACCESS_TOKEN,

  "game": "http://samples.ogp.me/163382137069945",

  "passed_users": "Some Arbitrary String",

  "score": "42"

}, { headers : {"Content-Type" : "application/x-www-form-urlencoded"}})

```

```javascript

assert.equal( gamesPassesCreateResponse.status, 200 )

```



Delete a games.passes object



```javascript

client.object_id( gamesPassesCreateResponse.body.id ).delete()

```