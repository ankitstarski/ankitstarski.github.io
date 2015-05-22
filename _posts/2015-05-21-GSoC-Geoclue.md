---
layout: post
title: "Sharing mobile GPS with PC"
---

This is my first post for [Planet GNOME](http://planet.gnome.org) regarding my 
[GSoC 2015](http://google-melange.com/gsoc/homepage/google/gsoc2015) 
[Project](https://wiki.gnome.org/Outreach/SummerOfCode/2015/Projects/Ankit_GPSSharing) under the
mentorship of [Zeeshan Ali (Khattak)](https://wiki.gnome.org/ZeeshanAli). 
This summer, I am working on a project to let GNOME users access GPS location (shared) 
from a GPS source. Android devices are among the most common GPS sources. So, a 
part of this project will be on Android. This android application will work as a 
location server for [Geoclue](http://freedesktop.org/wiki/Software/GeoClue/). 
This server will be discoverable via mDNS clients like [Avahi](http://en.wikipedia.org/wiki/Avahi_%28software%29) 
and [Bonjour](http://en.wikipedia.org/wiki/Bonjour_%28software%29). 

{% include image.html url="/media/2015-05-21-GSoC-Geoclue/location-sharing-apps.png" width="100%" description="Tentative design of Android application" %}

In a broad sense, Android application will use [ServerSocket](http://developer.android.com/reference/java/net/ServerSocket.html) 
to communicate with Geoclue. Each time Android [LocationListener](http://developer.android.com/reference/android/location/LocationListener.html) listens to a new Location, it will transmit that location data to Geoclue in form
of [NMEA sentence](http://www.gpsinformation.org/dale/nmea.htm#nmea). Geoclue takes 
that location information and passes it on as GPS location. Apart from the android application, 
GUI application for Geoclue to Geoclue location passing is to be developed.

#### Timeline followed by the project will be as follows:
<table><tbody>
<tr>
	<td style="min-width:150px">Week 1 and 2 </td>
  <td>Experiments with the code and implementation of a basic layout for new Geoclue location source. This Geoclue location source would be able to take in NMEA GGA sentences and convert into location object.</td>
</tr>
<tr>
	<td style="min-width:150px">Week 3 and 4 </td>
  <td>Geoclue Location source continued.</td>
</tr>
<tr>
	<td style="min-width:150px">Week 5 and 6 </td>
  <td>Working skeleton of Android application that will connect with Geoclue and send information.</td>
</tr>
<tr>
	<td style="min-width:150px">Week 7 and 8 </td>
  <td><em>Geoclue GUI</em> for Geoclue to Geoclue sharing.</td>
</tr>
<tr>
	<td style="min-width:150px">Week 9 and 10 </td>
  <td>Implementation of more seamless connectivity using <em>Android NSD</em> and <em>Avahi</em>.</td>
</tr>
<tr>
	<td style="min-width:150px">Week 11 and 12 </td>
  <td>Android application UI/UX improvements.</td>
</tr>
<tr>
	<td style="min-width:150px">Week 13 </td>
  <td>Testing, optimization, bug fixing and wrapping up.</td>
</tr>
</tbody></table>
