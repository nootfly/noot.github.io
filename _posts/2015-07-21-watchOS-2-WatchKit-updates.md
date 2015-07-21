---
layout: post
title:  "watchOS 2 WatchKit updates"
date:   2015-07-21 15:00:00+1000
categories: ios watchkit
tags: ios watchkit
---

There are many updates in WatchKit of watchOS 2. I will talk about five new enhancements.

#### Animations
Now you can use animateWithDuration function of WKInterfaceController to animate some properties of objects.
{% highlight c %} 
[self animateWithDuration:0.3 animations:^{
   [self.buttonSpacerGroup setHeight:100];
}];
{% endhighlight %}

#### Taptic engine
Developers can manipulate Taptic engine on the Apple watch. The API is quite simple. Only one function (playHaptic) can be used, and just 9 types Taptic can be played.
{% highlight swift %}
WKInterfaceDevice.currentDevice().playHaptic(WKHapticType.Notification)
WKInterfaceDevice.currentDevice().playHaptic(WKHapticType.DirectionUp)
WKInterfaceDevice.currentDevice().playHaptic(WKHapticType.DirectionDown)
WKInterfaceDevice.currentDevice().playHaptic(WKHapticType.Success)
WKInterfaceDevice.currentDevice().playHaptic(WKHapticType.Failure)
WKInterfaceDevice.currentDevice().playHaptic(WKHapticType.Retry)
WKInterfaceDevice.currentDevice().playHaptic(WKHapticType.Start)
WKInterfaceDevice.currentDevice().playHaptic(WKHapticType.Stop)
WKInterfaceDevice.currentDevice().playHaptic(WKHapticType.Click)
{% endhighlight %}

#### WatchConnectivity
WatchConnectivity framework is used to communicate between iPhone and Apple Watch. The most important class is WCSession. The delegate is used to receive messages from a counterpart.

This is an example to send a message.
{% highlight swift %}
override func willActivate() {
    super.willActivate()
    
    if (WCSession.isSupported()) {
        session = WCSession.defaultSession()
        session.delegate = self
        session.activateSession()
    }
}

@IBAction func sendMessage(){
  let applicationDict = ["message":"test"]
  do {
      try session.updateApplicationContext(applicationDict)
  } catch {
      print("error")
  }
}
{% endhighlight %}

This is an example to receive a message.
{% highlight swift %}
override func viewDidLoad() {
    super.viewDidLoad()
    
    if (WCSession.isSupported()) {
        session = WCSession.defaultSession()
        session.delegate = self;
        session.activateSession()
    }
}

func session(session: WCSession, didReceiveApplicationContext applicationContexString : AnyObject]) {
    let message = applicationContext["message"] as? String
    
    //Use this to update the UI instantaneously (otherwise, takes a little while)
    dispatch_async(dispatch_get_main_queue()) {
        self.label.text = "Last message: " + message!
    }
}
{% endhighlight %}


#### WKInterfacePicker
WKInterfacePicker is a new control in watchOS 2 and its value can be changed by spinning the digital crown. "focusForCrownInput" is the method to focus this control.

The below is the simplest example of WKInterfacePicker.
{% highlight c %}
NSMutableArray *items = [[NSMutableArray alloc] init];
for(int c = 1; c <= 60; c++) {
    WKPickerItem *item = [[WKPickerItem alloc] init];
    item.title = [NSString stringWithFormat:@"No %d", c];
    [items addObject:item];
}
[self.picker setItems:items];
{% endhighlight %}


#### Complication
The important class of complication is CLKComplicationDataSource class. Developers need to create a class to implement this delegate. There are 5 families of complications:  ModularSmall, ModularLarge, UtilitarianSmall, UtilitarianLarge, CircularSmall.
[Reference](http://www.sneakycrab.com/blog/2015/6/10/writing-your-own-watchkit-complications)

This is the code to return current complication.
{% highlight swift %}
func getCurrentTimelineEntryForComplication(complication: CLKComplication, withHandler handler: (CLKComplicationTimelineEntry?) -> Voi
   if complication.family == .CircularSmall {
       let template = CLKComplicationTemplateCircularSmallRingText()
       template.textProvider = CLKSimpleTextProvider(text: "8")
       template.fillFraction = Float(getCurrentHealth()) / 10.0
       template.ringStyle = CLKComplicationRingStyle.Cl
       let timelineEntry = CLKComplicationTimelineEntry(date: NSDatcomplicationTemplate: template)
       handler(timelineEntry)
   } else {
       handler(nil)
   }
}
{% endhighlight %}