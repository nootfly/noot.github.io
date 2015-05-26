---
layout: post
title:  "iOS Crash list"
date:   2015-04-30 13:00:00+1000
categories: ios
tags: ios
---

There are many ways to crash iOS apps. The below lists these cases.


#### 1. NSString stringByReplacingOccurrencesOfString
This app would crash if test is nil.
      
    NSString *returnString = @"aaaa [amount]";
    [returnString stringByReplacingOccurrencesOfString:
    [NSString stringWithFormat:@"[%@]", @"amount"] withString:test];

#### 2. UILocalNotification
If iPhone iOS version is below 8.2, this app would crash.

    UILocalNotification *localNotif = [[UILocalNotification alloc] init];
    localNotif.category = @"myCategory"; //available iOS8.0
    localNotif.alertTitle = @"test"      //available iOS 8.2

#### 3. Callback is nil
This app would crash if errorHandler is nil.

    failure:^(AFHTTPRequestOperation *operation, NSError *error) {
       errorHandler(error);
    };


#### 4. AFNetworking 1.3.1
This version is buggy and the app would crash when the same requests are submitted in a short time.

#### 5. Index beyond bounds

    NSArray *sample = [[NSArray alloc] init];
    NSString *test = sample[1];

    NSString *lastProductionYearStr = "test"
    NSString *year = [lastProductionYearStr substringToIndex:5];

#### 6. object cannot be nil when adding nil to an array

     UIViewController *controller = nil;
     [self addChildViewController:controller];

