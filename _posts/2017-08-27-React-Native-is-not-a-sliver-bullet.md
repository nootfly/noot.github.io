---
layout: post
title:  "React Native is not a sliver bullet"
date:   2017-08-27 18:03:00 +1000
categories: javascript
tags: javascript,rn
---

React Native (RN) is a good solution for some mobile apps, but it is not a sliver bullet for all mobile apps. Some may say they share 90% code between Android and iOS, but they do not mention some limitations of their apps. I think React Native is not good fit for small and medium size companies because those companies do not have resources to overcome some caveats of React Native. The reasons are as the below.

#### 1. React Native SDK is just a intersection of Android and iOS SDK.
<img src="/images/ios/rn1.png" alt="Drawing" style="width: 303px;height: 205px"/>

This means that React Native SDK is limited and it just caters for the common parts of both Android and iOS SDKs. Also some RN SDK APIs are just for one of platforms. Some may argue that we can write bridges in native code to do what RN cannot do. But the bridge code is not RN code.

#### 2. React Native SDK and documents alway fall behind the latest Android and iOS SDK.

This point is obvious as the latest Android and iOS are developed by Google and Apple first, then Facebook RN team will study the latest SDKs first and then add or update new RN code to catch up the latest SDKs. The documents are lagging behind too. Even for the most popular mobile OS versions, RN just cover a portion of those APIs of SDKs.

#### 3. React Native components are too simple for production, and it is lack of some core libraries and components.

It seems that Facebook just opened source for basic components. Some key components, such as routing, are missing. You need to find third party libraries to implement some basic functions. The process is painful as you need to compare and try different libraries. When you do a real project, you will find out these components are not enough for your projects. As a result, you need to spend lots of time to looking for some libraries or write you own components. As I know, Amazon rewrote all RN components which are ready for production. Amazon has a big team to maintain their components and does not relay on third party frameworks or libraries, which means Amazon built their own RN framework. For a small team or small companies, it is difficult to write those components.

#### 4. React Native apps' sizes are bigger than native apps, and they are not stable as native apps.

When you release your RN apps, you need to bundle RN framework on the top of native SDKs.
This makes RN apps' sizes bigger. On the other hand, your apps stability depends on RN framework since there is a middle layer between your apps and native SDK.

#### 5. React Native projects requires developers who understand not only javascript but also Android and iOS.

It does not mean that you can write good RN code when you only understand Javascript and React Native. Android and iOS have total different SDKs, design patterns, and UI elements. RN just covers the common of both platforms. You need to think through details of implementation on each platform and how to handle the difference between two SDKs. It's difficult to let Javascript, Android and iOS guys work together as they can have different opinions on some technique decisions.  

#### 6. React Native iOS depends on Apple's policy, also it is not the core business of Facebook. There is no guarantee that Facebook will support it forever.

RN on iOS is risky as Apple can shut it down or set some limitations anytime when it impacts Apple's app review process and policy. For example, Apple already forbade JSPatch [https://forums.developer.apple.com/thread/73856](https://forums.developer.apple.com/thread/73856).


In summary, in my opinion, RN is a good solution for some prototypes and some apps which have simple UI elements and navigations. RN is ok for big teams as they have resources to write their own components and maintain documents and conventions. RN is risky for big projects of small teams.
