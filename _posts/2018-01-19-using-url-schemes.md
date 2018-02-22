---
layout: post
title:  "Using URL Schemes to Communicate with Apps"
date:   2018-01-19 10:15:00 +1100
categories: ios
tags: ios
---

You can use URL schemes to open other apps. If you want to share data with other apps, you can use a shared keychina.

To open other apps, you need to do these steps.

1. You need to add a `URL Typ` in the `project -> targets -> info`. The `identifier` is the second app's bundle identifier, and the `URL Schemes` is used for the second app to open the first app. The `Role` can be `Editor`.

2. In the second app's `Info.plist`, you need to add the below setting. The `schema` is the value you set in the first step.
  ```
    <key>LSApplicationQueriesSchemes</key>
    <array>
    	<string>schema</string>
    </array>
  ```

3. After you finish the steps above, you can use this code to open the first app from the second app. The `schema` value is from the first step.
   ```swift
       let app2Url: URL = URL(string: "schema://")!
        
        if UIApplication.shared.canOpenURL(app2Url) {
            UIApplication.shared.open(app2Url, options: [:], completionHandler: { _ in
                
            })
        }
    ```

 To share data between the first app and the second app, you can follow the steps below:

 1. Two apps must be in the same keychain group. You need to enable `Keychina Sharing` and add two apps to the same keychain group in `project -> targets -> Capabilities`.

 2. The easy way to write data to the shared keychain is to use [UICKeyChainStore](https://github.com/kishikawakatsumi/UICKeyChainStore). For example,
    ```swift
      UICKeyChainStore.setString(ssoToken,forKey: "sso_token", service: "sharedService")
    ```
 3. You can get the data in the other app using code below:
    ```swift
       let ssoToken = UICKeyChainStore.string(forKey: "sso_token", service: "sharedService") 
    ```
          
   
   

    
