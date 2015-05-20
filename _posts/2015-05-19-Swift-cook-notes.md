---
layout: post
title:  "Swift cook book"
date:   2015-05-19 22:18:00+1000
categories: ios swift
tags: ios swift
---

####Get Current Date and Time 

    let date = NSDate()
    let calendar = NSCalendar.currentCalendar()
    let components = calendar.components(. CalendarUnitHouCalendarUnitMinute | .CalendarUnitMonth | . CalendarUnit| .CalendarUnitDay, fromDate: date)
    let hour = components.hour
    let minutes = components.minute
    let month = components.month
    let year = components.year
    let day = components.day





