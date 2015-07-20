---
layout: post
title: "mDNS integration"
---

This is a progress report for my GSoC project on [GPS Sharing](https://wiki.gnome.org/Outreach/SummerOfCode/2015/Projects/Ankit_GPSSharing).

#### What's done since the last blog post?

* Integration of mDNS in Android application using an opensource library for
  Java called [`JmDNS`](http://jmdns.sourceforge.net).

* Integration of Avahi inside Geoclue's Network NMEA source.

mDNS in Android is to be used for service broadcast, the service is supposed to
be named as `<hostname>-nmea-tcp-<accuracy_nick>`. The `accuracy_nick` will be a string, defining 
the accuracy of the source. The accuracy nicks can be found in Geoclue's [public api](http://www.freedesktop.org/software/geoclue/docs/geoclue-gclue-enums.html).
An example of service name could be `geoclue-nmea-tcp-exact`. The service type of the 
broadcasted service is _nmea._tcp at port number 10110 in local domain.

Geoclue's Network NMEA source discovers services with `_nmea._tcp` service type in 
the local domain. The Network NMEA source keeps a list of services sorted by their accuracy levels (highest first).
Whenever a client connects to geoclue, it checks for the service with highest accuracy level and tries to connect with it, 
which then starts the flow of NMEA GGA sentences from android device.

<br>

Note: This a work in progress and is not yet available in the main repository. Android application source can be accessed [here](https://github.com/ankitstarski/GeoclueShare). And Geoclue patches [here](https://bugs.freedesktop.org/show_bug.cgi?id=90974).

<hr/>
#### EDIT:

Instead of using service name for sharing accuracy with Geoclue, we are now using 
`mDNS` `TXT` records for sharing accuracy of the device.

