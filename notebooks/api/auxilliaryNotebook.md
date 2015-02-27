---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/portal/pages/7046/preview
apiNotebookVersion: 1.1.66
title: Auxilliary notebook
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
PAGE_ID = prompt("Please, enter ID of a page you wish to work with. This page must be created by you and it must be involved into conversation.")
```

```javascript
// Read about the Facebook RAML API at https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7965/versions/8129/contracts
API.createClient('client', '/apiplatform/repository/public/organizations/30/apis/7965/versions/8129/definition');
```

```javascript
API.authenticate(client,"oauth_2_0",{
  clientId: CLIENT_ID,
  clientSecret: CLIENT_SECRET,
  scope: [ "manage_pages", "read_page_mailboxes" ]
})
```

```javascript
ACCESS_TOKEN = $5.accessToken//access the 'accessToken' field of the response of the API.authenticate() cell
```

```javascript
accountsResponse = client.user_id("me").accounts.get()
```

```javascript
PAGE_ACCESS_TOKEN = null
accountsResponse = client.user_id("me").accounts.get()
for(var ind in accountsResponse.body.data){
  var item = accountsResponse.body.data[ind]
  if(PAGE_ID==item.id){
    PAGE_ACCESS_TOKEN = item.access_token
  }
}
```

```javascript
PAGE_ACCESS_TOKEN
```

```javascript
window.alert("PAGE_ACCESS_TOKEN = \n\n" + PAGE_ACCESS_TOKEN)
```