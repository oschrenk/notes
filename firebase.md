# Firebase

Firebase is an event driven database. Clients can write to the database and listen to chchanges via websockets.

Firebase is a document based database only serves a single document.

Instead if arrays, colections of ordered data are presented by objects instead of arrats. The keys are auto-generated and encode the timestamp and client id.

Because of its structure, data is strictly hierarchical, and thus Firebase has unique urls to serve data under a path.

So if for isntance your database is called `my-first-database` il will accessible via `https://my-first-database.firebaseio.com`. To then access for instance a top level `visitors` field you can manipulate `https://my-first-database.firebaseio.com/visitors`.



1. Create an application in Firebase Console

https://console.firebase.google.com/


2. Setup Authentication method

Go to https://console.firebase.google.com/u/0/project/my-firebase-cafebabe/authentication/providers

It offers various methods

1. Username/Password
2. Phone
3. Google
4. Facebook
5. Twitter
6. Github
7. Anonymous

Let's try Google first.

## Google Auth

Setup *Project public-facing name* under https://console.firebase.google.com/u/0/project/my-firebase-cafebabe/settings/general/

and the  e able the auth method.  it has a

open `quickstart-js/auth/google-redirect.html`
edit `quickstart-js/auth/google-redirect.js`
## How do I get `serviceAccountKey`?

1. Go to your Project
2. Click on the wheel to open _Project Settings_
3. Go to Service Accounts Tab
4. Click on _Generate New Private Key_ button

Where is

```
  firebase: {
    apiKey: "...",
    authDomain: "...",
    databaseURL: "...",
    projectId: "...",
    storageBucket: "...",
    messagingSenderId: "..."
  }
```

The easiest is to Project > Authentication. On your top-right. There is a link/button thing called "Web setup". click on that

* `apiKey`: Project > Project Settings > General > Web API Key
* `authDomain`:
* `messagingSenderId`  Project > Project Settings > Cloud Messaging > Sender ID

