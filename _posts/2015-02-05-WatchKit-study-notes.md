---
layout: post
title:  "WatchKit study notes"
date:   2015-02-05 15:50:00+1000
categories: ios watchkit
tags: ios watchkit
---

Reference:[Apple Watch Programming Guide](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html#//apple_ref/doc/uid/TP40014969-CH8-SW1), the images in the doc are from this guide.

### WatchKit Introduction
WatchKit is used to develop third-party apps on Apple Watch. Currently just Xcode6.2 beta supports WatchKit. The user interacts with WatchKit apps in the three ways. WatchKit apps cannot access Apple Watch sensors and GPU. Like Android wearable apps, WatchKit apps are bundled with iPhone apps. When iPhones are connected to Apple watches, WatchKit apps are installed on Apple watches automatically. 
Apple Watch has two sizes: 38mm and 42mm.

- **WatchKit Apps**: Your apps just like a normal iOS apps running on WatchKit. Users can interact with theses apps.

<img src="/images/watchkit/apps_2x.png" alt="Drawing" style="width: 180px;height: 200px"/>


- **Glances**: A glance is a read-only interface. Glances do not scroll, so they must fit on a sigle screen. A glance should not contain buttons, switches and other interactive controls. Tapping a glance launches your WatchKit app.

<img src="/images/watchkit/glances_2x.png" alt="Drawing" style="width: 180px;height: 200px"/>

- **Actionable Notifications**: Users can take actions for local and remote notifications.

<img src="/images/watchkit/notifications_2x.png" alt="Drawing" style="width: 180px;height: 200px"/>


### WatchKit App Architecture
Apps built for Apple Watch consists two parts: a WatchKit app and a WatchKit extension. The WatchKit app runs on the user's Apple Watch and the extension runs on the user's iPhone. The watchKit app just contains storyboard and resources but no code. The extension includes codes creating objects for display. 

![](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Art/app_communication_2x.png)

#### WatchKit App Life Cycle
WatchKit apps communicate with WatchKit extensions back and forth when users interact with Watchkit apps. The life cycle of launching an WatchKit app is as the following:
![](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Art/launch_cycle_2x.png)
The life cycle of an interface controller is as the following:
![](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Art/watch_app_lifecycle_simple_2x.png)

#### Guidelines for extension apps

- **Avoid using technologies that request user permission, like Core Location**
- **Do not use background execution modes for a technology**
- **Avoid performing long-running tasks with a technology**

Tasks mentioned above should let iOS apps to handle them. WatchKit extensions are considered as foreground extensions because they run only when the user interact with the corresponding WatchKit apps.

### App Essentials
The storyboard is used to create WatchKit app interfaces. Currently, dynamic views are not supported for WatchKit apps. Autolayout cannot be used in the WatchKit storyboard. The scene in the WatchKit storyboard is backed by interface controllers subclass. There are two types of interface controller:

- WKInterfaceController 
- WKUserNotificationInterfaceController

WatchKit provides groups to position items within the storyboard scenes. For horizontal position, there are three values: left, centre and right. For vertical position, top, centre and bottom are used. 

<img src="/images/watchkit/storyboard_layout_2x.png" alt="Drawing" style="width: 258px;height: 207px"/>

#### Updating the interface at runtime
Interface controller can do the following actions:

 - Set or update data values.
 - Change the visual appearance of objects that support such modifications.
 - Change the size of an object.
 - Change the transparency of an object.
 - Show or hide an object.

Interface controller just set values of controls but read values from them.

#### Interface Navigation
AppleKit supports two types of navigation styles. They are mutually exclusive.

- **Page-based**: The segues are used to connect different pages. Different sets of pages can be loaded by calling **reloadRootControllersWithNames:contexts:** method early in the **init** method.

- **Hierarchical**: When a screen is transitioned to a new screen using segues or by calling calling the **pushControllerWithName:context:** method. You cannot push a interface controller which does not exist in the storyboard.  

There are three ways to present interface controllers modally:

- Create a modal segue in your storyboard file.
- Call the **presentControllerWithName:context:** method to present a single interface controller modally
- Call the **presentControllerWithNames:contexts:** method to present two or more interface controllers modally using a page-based layout.

#### Interface Objects
WatchKit uses interface objects to organise screens. An interface object must extend WKInterfaceObject class or one of its subclasses. You are not allowed to alloc/init interface objects yourself. Interface objects are not views just proxy objects which can communicate with the actual views. Interface Objects include group, table, image, separator, button, switch, slider, label, date, timer, map, menu and menu item.

#### Tables
WatchKit just supports single-column table. You must use WKInterfaceTable class. The difference between WatchKit table and UITableView is:

- There is no data source and delegate for a WatchKit table
- WatchKit table uses row controllers to display cells. You can create customized row controllers for different display.

**To create and configure the rows of a table**

 1. Use the **setRowTypes:** or **setNumberOfRows:withRowType:** method to create the rows.
Both methods create the rows in your interface and instantiate each row’s corresponding class in your WatchKit extension. The instantiated classes are stored in the table and accessible using the **rowControllerAtIndex:** method.
 2. Iterate over the rows using the **rowControllerAtIndex:** method.
 3. Use the row controller objects to configure the row contents.

When the user touches a row of a table, the table:didSelectRowAtIndex: method of the interface controller is called.
Apples suggests that a table should show records whose quantity are less than 20.

#### Context Menus
Pressing Apple Watch screen with a small amount of force activates the context menu if it is created.

<img src="/images/watchkit/context_menu_2x.png" alt="Drawing" style="width: 168px;height: 207px"/>

A context menu is added in a storyboard. Menu items can be added in the storyboard or adding them dynamically by calling the **addMenuItemWithImage:title:action:** or **addMenuItemWithImageNamed:title:action:** method of your interface controller object. The context menu is dismissed when a item is touched and the corresponding method is called. The quantity of context menu items is less than 4.

### Glances
Glances are used to show important information of your WatchKit app. It is optional. The glance interface is managed by a custom WKInterfaceController object. An app can have only one glance interface controller.

**Glance Interface Guidelines**

- Design your glance to convey information quickly
- Focus on the most important data
- Do not include interactive controls in your glance interface
- Avoid tables and maps in your glance interface
- Be timely with the information you display
- Use the system font for all text

Normally, the main interface controller displays of the WatchKit app when the user touches a glance. But you can display a different interface controller by calling **updateUserActivity:userInfo:** method from your glance interface controller and implement the **handleUserActivity:** method in the main interface controller.


### Notifications
iOS apps decide whether to display notifications on Apple Watch. WatchKit provides a default notification interface that shows the alert message from the notification. If you want to show detail contents and action buttons, a custom interface is needed. There two types of interface for a notification.

- **The short-look interface**: This interface displays when the user first looks at a notification. 

<img src="/images/watchkit/shortlook_calendar_2x.png" alt="Drawing" style="width: 332px;height: 182px"/>


- **The Long-Look Interface**: The long-look interface is usually a scrollable screen associated with some action buttons.

<img src="/images/watchkit/longlook_calendar_2x.png" alt="Drawing" style="width: 479px;height: 703px"/>


#### Adding Action Buttons to Notifications
In iOS 8 and later, notification-generated alerts are required to register using **UIUserNotificationSettings** object, at the same time, a set of custom notification categories are registered too. The registration method is implemented in the iOS app delegate and called at launch time.

#### Responding to Taps in Action Buttons
When the user taps an action button for a notification, foreground and background actions are processed differently:

- Foreground actions launch your WatchKit app and deliver the ID of the tapped button to the **handleActionWithIdentifier:forRemoteNotification:** or **handleActionWithIdentifier:forLocalNotification:** method of your main interface controller.
- Background actions launch the containing iOS app in the background so that it can process the action. Information about the selected action is delivered to the **application:handleActionWithIdentifier:forRemoteNotification:completionHandler:** or **application:handleActionWithIdentifier:forLocalNotification:completionHandler:** method of the app delegate.

#### Managing a Custom Long Look Interface

WatchKit provides a static and a dynamic interface by default. The dynamic interface is optional and used to customize the display of your notification's content.

![](/images/watchkit/notification_static_dynamic_2x.png)

**The static interface will show when**
 
 - A dynamic interface is not available
 - Not enough power to warrant displaying the dynamic interface
 - Explicitly tell WatchKit not to display the dynamic interface

 WatchKit apps can have multiple notification interfaces using different categories.

#### Configuring the Static Notification Interface 

The rules for creating a static interface are as follows:

 - All images must reside in the WatchKit app bundle.
 - The interface must not include controls, tables, maps, or other interactive elements.
 - The interface's notificationAlertLabel outlet must be connected to a label. The label's contents are set to the notification's alert message. The text for all other labels does not change and is whatever is specified in your storyboard file.

#### Configuring the Dynamic Notification Interface
 When a notification comes, WatchKit display the right scene on Apple Watch and instantiate the corresponding **WKUserNotificationInterfaceController** subclass. The payload data is delivered to the notification interface controller using either the **didReceiveRemoteNotification:withCompletion:** or **didReceiveLocalNotification:withCompletion:** method. 

 In **didReceiveRemoteNotification:withCompletion:** or **didReceiveLocalNotification:withCompletion:** method, when calling the completion handler block

 - completionHandler(WKUserNotificationInterfaceTypeCustom): display the custom interface
 - completionHandler(WKUserNotificationInterfaceTypeDefault): display the static interface


