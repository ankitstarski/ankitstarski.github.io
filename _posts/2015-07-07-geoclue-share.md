---
layout: post
title: "Geoclue Share"
---

So, this post is about an Android application named 'Geoclue Share' which will be 
able to share location data from an android device to Geoclue device.

This is part of a GSoC 2015 project. You can read more about it [here]
(https://wiki.gnome.org/Outreach/SummerOfCode/2015/Projects/Ankit_GPSSharing).  

#### What's been done up till now?

* Created Geoclue module which will connect to the Location server for location
  data.
* Created a basic Basic Android application which sends location data to
  connected Geoclue client, whenever available.

#### How does it work?

As soon as the client connects to the application, it starts the GPS and starts
sending in location data in form of GGA sentences.

If there is no client the GPS remains off. This application keeps on running in
the background (as a Service) in order to listen for new clients.

{% include image.html url="/media/2015-07-07-geoclue-share/geoclue-share.jpg" description="A flow diagram for our Android application." %}

#### What's next?

The next phase of this development would be to use mDNS for service broadcasting.
This way connecting two devices would become more seamless.

Android side will use [NSD](https://developer.android.com/training/connect-devices-wirelessly/nsd.html) 
for service broadcasting. [Avahi](http://www.avahi.org/) is to be used a Geoclue end.

<br/>

Note: This a work in progress and is not yet available in the main repository. Android application source can be accessed [here](https://github.com/ankitstarski/GeoclueShare). And Geoclue patches [here](https://bugs.freedesktop.org/show_bug.cgi?id=90974).