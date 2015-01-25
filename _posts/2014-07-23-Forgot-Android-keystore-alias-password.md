---
layout: post
title:  "Forgot Android keystore alias password"
date:   2014-07-23 10:34:51+1000
categories: android
tags: android
---
1. List Android keystore file alias

         keytool -list -keystore /location/of/your/com.example.keystore
2. Reset key password

         keytool -keypasswd -alias myandroidkeyalias -new newpassword -keystore myAndroidKey.keystore
3. Also, keystore password can be changed too.

         keytool -storepasswd -keystore my.keystore