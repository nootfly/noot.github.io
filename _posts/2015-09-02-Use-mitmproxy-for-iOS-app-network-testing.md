---
layout: post
title:  "Use mitmproxy for iOS app network testing"
date:   2015-09-02 11:35:00+1000
categories: ios
tags: ios
---

[mitmproxy](https://mitmproxy.org/) is an interactive console program that lets developers monitor network traffic flow. It is very useful for iOS networking debugging and testing because you can inspect requests and responses of all network traffic on a real device or a simulator. I will list the steps to use this tool.

1. Install mitmproxy on a Mac

   use this command to install mitmproxy

       pip install mitmproxy

   After I run this command, and I run into "no attribute 'TLSv1_2_METHOD'" issue. I followed one instruction of this [issue](https://github.com/mitmproxy/mitmproxy/issues/705) and solved this problem. The approach is as the below.

       sudo mv /System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/OpenSSL /System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/_OpenSSL
        

2. Set proxy on a Mac
   
   After mitmproxy is installed, you can run `mitmproxy` to start the proxy sever. If you want to use this proxy server on a iOS simulator, you need to update your network setting on your mac. The proxy server is your mac IP address. 
   <img src="/images/mitmproxy/proxy_setting_mac.png" alt="proxy setting on mac" />

   Next visit [http://mitm.it](http://mitm.it) to install a certificate, then you can see all network traffic including iOS simulators on a mitmproxy console.

3. Set proxy on an iPhone
   
   First go to iPhone WIFI setting, and then set your proxy server

   <img src="/images/mitmproxy/iphone_setting.jpg" alt="proxy setting on mac" />

   After the proxy server is set, and then open Safari and visit [http://mitm.it](http://mitm.it). Install a profile by clicking the Apple icon when you can see the screen below.

   <img src="/images/mitmproxy/mitmproxy_website.jpg" alt="mitmproxy website" />
   <img src="/images/mitmproxy/install_profile.jpg" alt="install profile" /> 

   After you have done the steps above, you can see all iPhone network traffic on a mitmproxy console.  
   <img src="/images/mitmproxy/mitmproxy.png" alt="mitmproxy" /> 

