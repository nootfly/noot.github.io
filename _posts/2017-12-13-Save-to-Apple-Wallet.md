---
layout: post
title:  "Save to Apple Wallet"
date:   2017-12-13 15:03:00 +1100
categories: ios
tags: ios
---

Adding a loyalty card to Apple wallet involves many steps. You need a server to provide Rest APIs for Apple wallet, and your server sends silent push notifications to devices.

1. Set the Pass Type Identifier and Team ID on the developer portal following [this link](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/PassKit_PG/YourFirst.html#//apple_ref/doc/uid/TP40012195-CH2-SW1)

2. Build web services based on Apple's [PassKit Web Service Reference](https://developer.apple.com/library/content/documentation/PassKit/Reference/PassKit_WebService/WebService.html)

3. If you use nodejs, you can use [node-passbook](https://github.com/assaf/node-passbook) to generate passes. 

4. According to [Wallet Developer Guide](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/PassKit_PG/Updating.html#//apple_ref/doc/uid/TP40012195-CH5-SW1), your server needs to maintain three tables: Device table, Passes table and Registrations table.

5. The push notification certificate is the pass certificate, and the content of silent push notications should be empty. If you use nodejs, you can use [node-apn](https://github.com/node-apn/node-apn) to send notifications.
```javascript
  let note = new apn.Notification({
      
        
    });
```    




