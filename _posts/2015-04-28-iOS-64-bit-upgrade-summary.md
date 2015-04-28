---
layout: post
title:  "iOS 64-bit upgrade summary"
date:   2015-04-28 13:00:00+1000
categories: ios
tags: ios
---

After June 1, 2015, all apps submitted to the App Store must include 64-bit support. For existing 32-bit projects, the upgrade must be done before June 1, 2015. So I did the upgrade and found something we need to pay attention to. There are some differences between iOS 7 and iOS8. Some layouts change a little bit in iOS 8 again iOS 9. The upgrade actually includes four parts: 1)64-bit support 2)iOS 8 support 3)iPhone 6 and iPhone 6 plus support 4)build server upgrade


### 64-bit upgrade
64-bit upgrade include project setting update, code update, third-party libraries update and test framework update.

#### 1. project setting update
We need to update project Architectures to:
      
      $(ARCHS_STANDARD)

#### 2. Code update

- Update int to NSInteger
- Update [NSString stringWithFormat:@"%i"] to [NSString stringWithFormat:@"%li"]  
- Update codes Xcode6.3 complains

#### 3. Third-party libraries update
Some old libraries do not support 64-bit. Some libraries latest versions support 64-bit without changing API, which is very lucky. Some libraries have update API a lot, which is very painful if the project relies those libraries heavily, such as RestKit. Facebook iOS SDK changed dramatically from version 3 to version 4. Google Analytics iOS SDK changed a lot from version 1 to version 3.

#### 4. Test framework update
GHUnitIOS has a weired problem about mapping. It takes time to fix this. 


### iOS 8 upgrade
Basically iOS 7 project should work on iOS 8, but it does not work really well. I've found some issues with iOS 8, while those issues do not exist on iOS 7.

#### 1. View background
Some view default background is not clear and white, but on iOS 7, it is clear. So we need to set [UIColor clearColor]

#### 2. UIButton image issue
setImageEdgeInsets function has different behaviors on iOS 7 and iOS 8.

#### 3. Popover content size and presented view controller content size issue
On iOS 8, these sizes are different with iOS 7.

#### 4. Popover content size and presented view controller content size issue
This is Apple's fault. They introduced new Popover and Alert API on iOS 8, but they broke old iOS 7 API. The size of presented view controller even has different between real devices and simulators on the same iOS version.


### iPhone 6 and iPhone 6 plus support
Add new launch images and icons for two new devices and fix the hard code width for iPhone 5 and iPhone 4.

### Build server upgrade
We need to install Xcode 6 and update build command tool and do testing.

### Other things considered
During the upgrade, I found the server does not support 64-bit. This breaks the app because "NSIntegerMax" is used to send to the server. After I upgraded the app to 64-bit, I need to update this code not to send 64-bit Integer to the server.

### Summary
64-bit upgrade seems very easy at the beginning. Many strange things happened during the upgrade. Also, the regression test is required for this non-functional upgrade because you do not know what would happen.



