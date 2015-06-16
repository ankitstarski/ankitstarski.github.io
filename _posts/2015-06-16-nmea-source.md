---
layout: post
title: "Network location sharing"
---

So, this post is about a new Geoclue feature that will let Geoclue users share
location data. Location data will be sent from a GPS enabled device to a Geoclue device over local network. 
This is part of a GSoC 2015 project. You can read more about it [here]
(https://wiki.gnome.org/Outreach/SummerOfCode/2015/Projects/Ankit_GPSSharing).  

#### What will this module do?

Using `GClueNMEASource`, Geoclue will be able to receive and parse location data sent 
from a GPS enabled network device. For this we'll have to create a location service on 
the GPS device that would let it send location data in the form of [GGA sentences]
(http://www.gpsinformation.org/dale/nmea.htm#GGA). `GClueNMEASource` uses `libgio`'s asynchronous 
GSockets for network communication purpose.

#### Protocol

1. Since the GPS device will be acting as Location server, Geoclue will first need to 
find and connect with it. For this we'll be using mDNS service on the GPS device 
and detect that service on the Geoclue end.
2. Once the connection between `GClueNMEASource` and location server is established, location server 
will firstly send the size of GGA sentence as 32-bit unsigned integer in network 
byte order.
3. After sending the size of GGA sentence, location server will send the GGA sentence containing 
location data.
4. This will be repeated every-time new location data is available on GPS device 
and connection between the devices is alive.

#### What's been done at Geoclue end?

1. Geoclue is currently able to connect with a dummy location server.
2. It can successfully receive GGA sentences and parse it into a Location object.

#### What might a dummy server look like?

Following is the python implementation for a dummy location server which continuously sends
pseudo location data in the form of GGA sentences:

<div class="highlight"><pre><code class="python">#!/usr/bin/python
import socket, struct
import time, math
import random, re

class DummyNMEAServer:
  """ This is a fake NMEA Sentence server. """

  HOST = ''
  PORT = 12345

  def __init__(self):
    self.socket = socket.socket()
    self.c = None

  def checksum(self, sentence):
    """ Source: http://code.activestate.com/recipes/576789-nmea-sentence-checksum/ """
    if re.search("\n$", sentence):
        sentence = sentence[:-1]

    calc_cksum = 0
    for s in sentence:
        calc_cksum ^= ord(s)

    """ Return the nmeadata, the checksum from
        sentence, and the calculated checksum
    """
    return format(calc_cksum, 'x')

  def run(self):
    """ Sends dummy location info of coordinates moving around (0, 0)
      in circular pattern """
    s = self.socket
    s.bind((DummyNMEAServer.HOST, DummyNMEAServer.PORT))
    s.listen(5)
    while True:
      c, addr = s.accept()
      self.c = c
      print 'Got connection from', addr
      lat = lon = 0
      cy = cx = 0.0
      r = 4.3
      t = 0
      latS = lonS = ""

      while True:
        for i in range(360):
          lat = cx + math.cos(i / 180.0 * math.pi) * r
          lon = cy + math.sin(i / 180.0 * math.pi) * r

          t = time.strftime("%H%M%S", time.gmtime(time.time()))

          latS = 'N' if lat >= 0 else 'S'
          lonS = 'E' if lon >= 0 else 'W'

          print t
          gga = "$GPGGA,%s,%02d%06.3f,%s,%03d%06.3f,%s,1,08,0.9,545.4,M,46.9,M,,"%(
            t, abs(lat), abs((lat-int(lat))*60), latS,
            abs(lon), abs((lon-int(lon))*60), lonS)
          gga = gga + "*" + str(self.checksum(gga))
          size = struct.pack("!L",len(gga))
          c.send(size)
          print(len(gga))
          print gga
          c.send(gga)
          time.sleep(random.randint(1,10))
    c.close()       

app = DummyNMEAServer();
app.run();</code></pre></div>

{% include image.html url="/media/2015-06-16-nmea-source/where-am-i.png" description="A screenshot of demo app while receiving location from dummy server" %}
