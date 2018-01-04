# Firebase

How do I get `serviceAccountKey`?

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

