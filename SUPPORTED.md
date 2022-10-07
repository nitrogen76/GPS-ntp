A note about gpsd and supportability:

Not all GPS modules are created equal.  In fact, many of the "BIG NAMES" commit grevious sins as far as following standards go.

Check out the gpsd [HALL OF SHAME](https://gpsd.io/hall-of-shame.html) to see what I mean.

In general, look at the [gpsd compatibility page](https://gpsd.io/hardware.html) for information on what to get.

Also, you want to insure that anything you get exposes PPS out for good timing information.  USB alone isnt' good enough, becaue NMEA sentences alone do not give you timing information, and USB's latency tends to be too unreliable alone for this.

Luckelly, Chronyd supports using a pps signal to lock to the GPS for nearly-nanosecond reliability)

My suggestion:

Get anything from ublox via sparkfun.

* [MAX-M10S](https://www.sparkfun.com/products/18037) would be a good low-cost option
* [NEO M9N](https://www.sparkfun.com/products/17285)
* [ZED-F9T](https://www.sparkfun.com/products/18774) would be excellent, but it's expensive.
* [GPS-RTK](https://www.sparkfun.com/products/16481) another expensive option
* [Counterfeit NEO-6M GPS](https://www.amazon.com/dp/B07P8YMVNT?th=1) works well enough, but I hate supporting piracy.
