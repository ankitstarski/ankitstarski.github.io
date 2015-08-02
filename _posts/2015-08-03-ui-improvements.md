---
layout: post
title: "UI/UX improvements"
---

This is a progress report for my GSoC project on [GPS Sharing](https://wiki.gnome.org/Outreach/SummerOfCode/2015/Projects/Ankit_GPSSharing).

#### What's done since the last blog post?

* UI/UX improvements in Android application.

* Fixes in Geoclue's Network NMEA source.

Android application now displays the level of accuracy provided by the device. It also displays the 
number of clients connected to the android device at any point of time. The Android application 
prompts the user to change location settings, if the GPS is not enabled.

{% include image.html url="/media/2015-08-03-ui-improvements/ui-comparison.png" description="Image showing mock-up and the actual UI side by side." %}


<br>

Note: This a work in progress and is not yet available in the main repository. Android application source can be accessed [here](https://github.com/ankitstarski/GeoclueShare). And Geoclue patches [here](https://bugs.freedesktop.org/show_bug.cgi?id=90974).