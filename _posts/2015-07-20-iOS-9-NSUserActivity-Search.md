---
layout: post
title:  "iOS 9 NSUserActivity Search tricks"
date:   2015-07-20 14:57:00+1000
categories: ios
tags: ios
---

iOS 9 introduces NSUserActivity Search. The API is very easy, but there are some tricks. The following solutions are tested on iOS 9 beta 3.

- On iOS 9 beta 3, NSUserActivity search only works on a real device. Update: On iOS 9 beta 4, NSUserActivity search can work on a simulator.

- Need to set NSUserActivity to current activity's userActivity property.
Otherwise the search result would not show. Reference: [StackOverflow](http://stackoverflow.com/questions/30836398/make-app-activities-and-states-searchable-by-using-nsuseractivity)
{% highlight swift %}       
self.userActivity = userActivity
{% endhighlight %}

- If you want to show a image and description in the search result. You need to do as the following:
{% highlight swift %}
let attributeSet = CSSearchableItemAttributeSet(itemContentType: "image" as String)
attributeSet.contentDescription = "This is an entry all about activity search"
attributeSet.thumbnailData = UIImagePNGRepresentation(UIImage(named: "ben")!)
userActivity.contentAttributeSet = attributeSet
{% endhighlight %}

The below is the workable code of an iOS NSUserActivity search on iOS 9 beta 3.
{% highlight swift %}
  let userActivity = NSUserActivity(activityType: "noot.test")
 userActivity.title = "Friends List";
 userActivity.keywords = Set(["fridend", "list", "noot"])
 userActivity.userInfo = ["action": "friends list"];
 userActivity.eligibleForSearch = true
 userActivity.eligibleForPublicIndexing = true
 userActivity.expirationDate = NSDate().dateByAddingTimeInterval(60*60*24)
 let attributeSet = CSSearchableItemAttributeSet(itemContentType: "image" as String)
 attributeSet.contentDescription = "This is an entry all about activity search"
 attributeSet.thumbnailData = UIImagePNGRepresentation(UIImage(named: "ben")!)
 userActivity.contentAttributeSet = attributeSet
 self.userActivity = userActivity
 userActivity.becomeCurrent()
{% endhighlight %}

