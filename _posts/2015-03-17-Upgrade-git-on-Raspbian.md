---
layout: post
title:  "Upgrade git on Raspbian"
date:   2015-03-17 14:56:00+1000
categories: git
tags: git
---

If the raspbian version is wheezy, then follow the steps below:

1. Edit sources.list

       sudo vi /etc/apt/sources.list

   add one line:

       deb http://http.debian.net/debian wheezy-backports main

2. apt-get
     
       sudo apt-get -t wheezy-backports install git     
         