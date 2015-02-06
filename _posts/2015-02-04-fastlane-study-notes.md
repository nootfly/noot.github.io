---
layout: post
title:  "Fastlane study notes"
date:   2015-02-04 17:00:00+1000
categories: ios ci
tags: ios ci
---

###Fastlane study
According to fastlane document, fastlane is a tool which can run your deployment pipelines for different environments. It can connect with other fastlane tools. Fastlane tools are listed below:

 1. snapshot  - it can capture iOS simulators' screen shots automatically.

 2. frameit   - it can add iPhone or iPad frames to screen shots.

 3. sigh      - it can create a new provision file using command line.

 4. deliver   - it is used to deliver an ipa file to testflight or appstore.

 5. produce   - it can create new iOS apps on iTunes Connect and Dev Portal using your command line


 Basically, fastlane creates pipeline scripts to use produce, deliver, sigh and snapshot tools. For example:

          lane :appstore do
             produce({
                      produce_username: 'felix@krausefx.com',
                      produce_app_identifier: 'com.krausefx.app',
                      produce_app_name: 'MyApp',
                      produce_language: 'English',
                      produce_version: '1.0',
                      produce_sku: 123,
                      produce_team_name: 'SunApps GmbH' # only necessary when in multiple teams
             })

             deliver
    end
Different lanes can be defined for testing, testflight and appstore. To launch the appstore lane run

          fastlane appstore

### Fastlane experience
Next I will talk about my experience when I use fastlane to deploy a new app to testflight.

####snapshot
Snapshot cannot support Xcode5.1.1. It only works for Xcode6.1 so far.
It can generate different iOS simulators' screen shots easily. The screen shots can be defined by recording behaviors in Instruments.

####produce
This tools couldn't work on my macbook due to version mismatch error with phantomjs. I submitted an issue to the author. This issue was solved quickly. It can create an app in AppDev centre and iTunes connect.

####sigh
It can create a provision file on the Apple development portal successfully using command line.

####shengzhen
This tools is used to build an ipa file using command line. It could generate an ipa file successfully when a right provision is chosen in an project settings. Many problems would meet when using this tool. The command below is very useful for investigating problems.

    ipa build --verbose

####deliver
It is very important tool for fastlane. But it could not work very well with shengzhen. The error would be seen many times. I'm still investigating whether it is a problem of this tool or the provision file.

    ERROR ITMS-90161: "Invalid Provisioning Profile


### Summary
fastlane is a promising tool for iOS CI. It can be easily integrated with Jenkins as a step of build process. But it still have many problems when using in a real project, and these problems could not be searched in development communities easily. So it may be not mature enough for production yet. We should watch it closely and we can use some of tools in our projects. 

Reference: [https://github.com/KrauseFx/fastlane](https://github.com/KrauseFx/fastlane)           
