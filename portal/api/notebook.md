The API notebooks are all executable! Hit "enter" in any code cell to execute it (and all cells before it that have not executed yet), or scroll to the bottom of the notebook and click "Play notebook". For more information, see [http://apinotebook.com](http://apinotebook.com).

#Considerations

- In order to run these notebooks you will need a Facebook account. Most notebooks ask you to authorize the client application to access to your account at some point of the excecution.
- You will also need to register a facebook application at the facebook developers site (see the "App" menue). Redirect URI of the application must be set to https://api-notebook.anypoint.mulesoft.com/authenticate/oauth.html. All the notebooks ask you for APP_ID and APP_SECRET in the very beginning.
- In order to run the "Page, conversations, milestone, offer" you must create a special page. After the page is created, you must involve it into conversation i.e. send a private message to it. Note that before launching this notebook you must obtain a PAGE_ACCESS_TOKEN as a result of launching the [auxilliary notebook](https://anypoint.mulesoft.com/apiplatform/popular/#/portals/apis/7965/versions/8129/pages/7046).
- Some methods require you to be the application's admin.
- The application must be set the "Games" category in order to execute methods related with achievments and scores. See the "App Details" tab.
- Those notebooks which deal with achievments ask for an URL which points to a valid achievment definition. The example of such definition is:​
<head prefix="og: http://ogp.me/ns# fb: http://ogp.me/ns/fb# book: http://ogp.me/ns/book#">
    <title>Notebook test achievement v0.2</title>
        <meta property="fb:app_id" content="APP_ID" />
        <meta property="og:type" content="game.achievement" />
        <meta property="og:description" content="This achievment is used to run 'facebook' API Notebook" />
        <meta property="og:url" content="PLACE THE URL HERE" />
        <meta property="game:points" content="15" />
        <meta property="og:title" content="Notebook Test Achievement" />
        <meta property="og:image" content="https://s-static.ak.fbcdn.net/images/devsite/attachment_blank.png" />
    </head>
<body>
    It's my sample achievement for the 'facebook' API Notebook v2.<br/> See the page code.
</body>

- Your facebook account must have at least one video, event and friendlist in order to execute methods related with these objects.
- Some notebooks require your account to have at least one page.
- The "App part 2, group part 1" notebook requires you to enter a valid facebook user_id in the very beginning. It will be added to lists of members and admins of created groups. You may use your own user_id.
