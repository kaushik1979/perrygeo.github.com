---
date: '2007-05-27 11:31:48'
layout: post
slug: python-gpsd-bindings
status: publish
title: Python gpsd bindings
wpid: '13'
---

If you want to get a linux/unix machine talking to your GPS unit, most likely you'll be using [gpsd](http://gpsd.berlios.de/). There are many great apps that build off of gpsd such as kismet and gpsdrive. 

Installing gpsd on debian/ubuntu systems is as simple as 
    
    sudo apt-get install gpsd gpsd-clients

You should be able to connect your gps via serial port and start a gpsd server 
    
    sudo gpsd /dev/ttyS0

The gpsd server reads NMEA sentences from the gps unit and is accessed on port 2947. You can test if everything is working by running a pre-built gpsd client such as xgps.

This is very useful for situations where you need lower-level access to the gps data; for logging your position to a postgres database for example. The debian packages (and most others I'm assuming) come with gps.py, a python interface to gpsd allowing you to pull your lat/long from the gps in real time. This opens the door for all sorts of neat real-time gps apps.



> 
>     import gps, os, time
>     
>     session = gps.gps()
>     
>     while 1:
>         os.system('clear')
>         session.query('admosy') 
>         # a = altitude, d = date/time, m=mode,  
>         # o=postion/fix, s=status, y=satellites
>     
>         print
>         print ' GPS reading'
>         print '----------------------------------------'
>         print 'latitude    ' , session.fix.latitude
>         print 'longitude   ' , session.fix.longitude
>         print 'time utc    ' , session.utc, session.fix.time
>         print 'altitude    ' , session.fix.altitude
>         print 'eph         ' , session.fix.eph
>         print 'epv         ' , session.fix.epv
>         print 'ept         ' , session.fix.ept
>         print 'speed       ' , session.fix.speed
>         print 'climb       ' , session.fix.climb
>         
>         print
>         print ' Satellites (total of', len(session.satellites) , ' in view)'
>         for i in session.satellites:
>             print '\t', i
>     
>         time.sleep(3)
>         
>     



... which gives you a simple readout to the terminal every 3 seconds.

![](/assets/img/gpsd_python.jpg)

Obviously there are much more interesting applications for this ( logging data to postgis, displaying real-time tracking data in QGIS via a python plugin, etc). But this is a good start for any python based app.
