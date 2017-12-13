---
layout: post
title:  "Save to Android Pay"
date:   2017-12-12 18:03:00 +1000
categories: android
tags: android
---

Adding a loyalty card to Android Pay is not straightforward at all. You need to follow some steps and ask help from Google Wallet tech support. You can save a loyalty card to Android Pay through an app or a website, but you only can update the card through Rest APIs. I list the steps what I have done.

1. Follow [basic step for Android](https://developers.google.com/save-to-android-pay/guides/basic-setup-android), you need to prive your app package name, your SHA1 fingerprint, and your issuer ID which Google can create one for you in order to whitelist your app. This step is the most important step. You can now access the Merchant Center at [https://wallet.google.com/merchant/walletobjects](https://wallet.google.com/merchant/walletobjects).

2. Accept the TOS and creating the authentication credential
[This page](https://developers.google.com/save-to-android-pay/guides/basic-setup-web) provides a straightforward step-by-step process to generating the authentication credential, during which you'll have the opportunity to click-to-accept the API Terms of Service. The correct role of service account would be "Project -> Service Account Actor". However, any role that provides more access such as "Project -> Owner" or "Project -> Editor" would work as well.

3. Associate your authentication credential with your account
At the end of the setup process you'll receive a Google-generated service account address in the format XXXXXXXXXXX@developer.gserviceaccount.com. In the merchant dashboard, you must add this address to your account by clicking on the “Share” button and adding it with “Edit” permissions. From this point on, you can access the API using your authentication credential.

4. You can setup the Android app demo available [here](https://developers.google.com/save-to-android-pay/samples/quickstart-android). You need to update your issuer ID, loyalty class ID and loyalty object ID. You can now save a loyalty object to Android Pay.

5. If you want to update the loyalty object, the only way is to update it from a server. You can setup the Java server demo available [here](https://developers.google.com/save-to-android-pay/samples/quickstart-java). You need to update the version when you want to update or patch the loyalty object.


