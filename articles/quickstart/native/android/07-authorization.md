---
title: Authorization
description: This tutorial will show you how to use the Auth0 authentication API in your Android project to create a custom login screen.
seo_alias: android
budicon: 500
---

This step demonstrates how to use Auth0 to create access roles for your users. With access roles, you can authorize or deny content to different users based on the level of access they have.

<%= include('../../../_includes/_package', {
  org: 'auth0-samples',
  repo: 'auth0-android-sample',
  path: '07-Authorization',
  requirements: [
    'Android Studio 2.3',
    'Android SDK 25',
    'Emulator - Nexus 5X - Android 6.0'
  ]
}) %>__

## Before Starting

Be sure that you have completed the [Login](/quickstart/native/android/00-login) quickstart.

## Create A Rule To Assign Roles

First, you need to create a rule that assigns your users either an `admin` role, or a simple `user` role. To do so, go to the [new rule page](${manage_url}/#/rules/new) and select the "*Set Roles To A User*" template, under *Access Control*. Then, replace the default script with the contents of the following snippet:

```js
function (user, context, callback) {

  //Define the name of the claim. Must look like a url:
  //Have 'http' or 'https' scheme and a hostname other than
  //'auth0.com', 'webtask.io' and 'webtask.run'.
  var claimName = 'https://access.control/roles';

  //Check if the email has the 'admin.com' domain and give the 'admin' role.
  //Otherwise, keep a default 'user' role.
  var roles = ['user'];
  if (user.email && user.email.indexOf('@admin.com') > -1) {
      roles.push('admin');
  }
  //Set the role claim in the id_token
  context.idToken[claimName] = roles;

  callback(null, user, context);
}
```

The rule will run every time a user attempts to authenticate. If the user has a valid `email` and the email's domain is `admin.com`, the user will be granted the `user` and `admin` roles. On any other case, the user will be granted a simple `user` role. This new claim is saved in the *id_token* under the `https://access.control/roles` name, which you will have to check on your app.

::: note
Feel free to customize the required conditions and the different roles to grant on each of them. You can define more roles other than `admin` and `user`, depending on your product requirements. There is a restriction on the naming of the claims, please read [this article](https://auth0.com/docs/rules/current#hello-world) for more context about Rules.
:::


## Test the Rule in Your Project

Once you have the user credentials (as explained in the [Login](/quickstart/native/android/00-login) tutorial), you can save and access them at any point.

The *id_token* is a [JWT](https://auth0.com/docs/jwt) that holds several claims, like the one with the roles you set. By using a *JWT decoding library* like [this one](https://github.com/auth0/JWTDecode.Android) you will be able to obtain the roles and perform the access control.

```java
// app/src/main/java/com/auth0/samples/activities/MainActivity.java
JWT idToken = new JWT(CredentialsManager.getCredentials(this).getIdToken());
final List<String> roles = idToken.getClaim("https://access.control/roles").asList(String.class);

if (!roles.contains("admin")) {
  // User is not authorized
} else {
  // User is authorized  
}
```

## Restrict Content Based On Access Level

At this point, you are able to distinguish the users roles in your app and authorize or deny (depending on the user) access to a certain feature. In the sample project those users with the `admin` role will be able to access the "Settings Activity".
