---
layout: post
title:  "UIWebView WKWebView SFSafariViewController"
date:   2015-07-23 10:50:00+1000
categories: ios
tags: ios
---

In WWDC 2014, Apple introduced WKWebView for UIKit and AppKit. WKWebView uses Nitro JavaScript engine. In WWDC 2015, SFSafariViewController was introduced. I will give a simple summary about the differences among them.

##### Performance 
According to [Init labs](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview) and [NSHipster](http://nshipster.com/wkwebkit/), WKWebView has better rendering performance in WebGL and 2D and 3D graphic.

#### Usage
UIWebView and WKWebView are just views and they can be used with other views. But SFSafariViewController is a controller and basically it cannot be used with other views. Even we can put it in a container, it would look weird as it has a address bar. So normally we just present it as a new view controller.

#### UIWebView and WKWebView
UIWebView belongs to UIKit and WKWebView belongs to WebKit. There is an interface builder component for UIWebView, but there is not one for WKWebView now. UIWebView can still be used for iOS 7 and older, and its usage is simpler than WKWebView. WKWebView is just for iOS 8 and newer. For simple web pages, UIWebView is still an option. WKWebView has a similar functionality with UIWebView, but it is more powerful. In iOS 8, Apple introduced 14 classes and 3 protocols compared to one class and one protocol with UIWebView & UIWebViewDelegate. Mobile Safari cookies and Auto Fill are unavailable for UIWebView and WKWebView due to security reasons. 


An example of WKWebView
{% highlight c %} 
WKWebViewConfiguration *theConfiguration = [[WKWebViewConfiguration alloc] init];
WKWebView *webView = [[WKWebView alloc] initWithFrame:self.view.frame configuration:theConfiguration];
webView.navigationDelegate = self;
NSURL *nsurl=[NSURL URLWithString:@"https://news.ycombinator.com"];
NSURLRequest *nsrequest = [NSURLRequest requestWithURL:nsurl];
[webView loadRequest:nsrequest];
[self.view addSubview:webView];
{% endhighlight %}

An example of UIWebView
{% highlight c %}
let myWebView:UIWebView = UIWebView(frame: CGRectMake(0, 0, UIScreen.mainScreen().bounds.width, UIScreen.mainScreen().bounds.height))
myWebView.loadRequest(NSURLRequest(URL: NSURL(string: "https://news.ycombinator.com")!))
self.view.addSubview(myWebView)
{% endhighlight %}

#### SFSafariViewController
This is a mini Safari which can be embedded in your apps. This controller can share cookies and auto fill data with the mobile Safari. It has "Done" button and can return the app after this button is clicked. One thing noticed is that the address in the address bar cannot be changed.

An example of SFSafariViewController
{% highlight c %}
if let url = NSURL(string: "https://news.ycombinator.com/") {
   let vc = SFSafariViewController(URL: url, entersReaderIfAvailable: true)
   vc.delegate = self
   presentViewController(vc, animated: true, completion: nil)
}
{% endhighlight %}

#### Issue with WKWebView
Mobile Chrome still uses UIWebView [Reference](https://code.google.com/p/chromium/issues/detail?id=423444). Some think that WKWebView API is still unfinished.

