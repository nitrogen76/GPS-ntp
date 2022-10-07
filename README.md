# GPS-ntp
Example config files for GPS NTP server

Here's how I created a nanosecond-accurate gps master ntp clock using chronyd, a raspberry pi, and a NEO-6M ublox clone.

You can create a GPS master that's "good enough" with millisecond accuracy with just the NEO-6M and a USB cable.
These instructions will tell you how to do either.

FIRST: get a NEO-6M GPS chip.  Any GPS that is supported by gpsd, or speaks NMEA sentences will work.
NMEA sentences take more latency, so using one whose binary format is compatible helps if you aren't doing the high-power
setup.

I bought this one here:
https://www.amazon.com/dp/B07P8YMVNT?th=1
but any chip with a USB out or a tty out, plus a PPS out will work.

IF you're going for the nanoscale one, I'm going to assume you're using a raspberry Pi model B, but will show you which pins matter if you're using something else.

If you're using a RPI, and want to use the pps nanoscale option:
* add this line to /etc/modules.conf:
`pps-gpio`

* Add this line to your /boot.txt:<br>
`dtoverlay=pps-gpio,gpiopin=18`

* If you want to use the tty output (which I didn't, for ease of use) add these lines to /boot.txt<br>
`enable_uart=1
init_uart_baud=9600`

I just use the USB connection, it's better quality.

Now, you'll have to do some soldering.  Solder the header onto the GPS chip.  You really only need two pinouts, GND and PPS.
GND goes to pin 3 on the rpi pinout
PPI goes to GPIO 18 (which is pin 6, i know, its weird)
Refer to the rpi B pinout here, or whatever you're using.
https://www.etechnophiles.com/wp-content/uploads/2020/12/R-Pi-3-B-Pinout-768x572.jpg

Boot up your rpi, and now let's set up gpsd.
I have a sample config file in https://github.com/nitrogen76/GPS-ntp/blob/master/gpsd that goes in /etc/default/gpsd

Now, set up chronyd.  I have a sample config file for that here: https://github.com/nitrogen76/GPS-ntp/blob/master/chrony.conf

You know it works if you see the following output for "chronyc sources"
<pre>
`MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
#- GPS                           0   4   377    12    +51ms[  +51ms] +/-  163ms
#* PPS                           0   4   377    12   +558ns[ +739ns] +/-  312ns
^- lofn.fancube.com              2  10   377   146  -1415us[-1415us] +/-   42ms
^- sg.ntp.tlercher.de            2  10   377    92    +12ms[  +12ms] +/-  116ms
^- ntp18.doctor.com              2   9   377   220   +369us[ +370us] +/-   46ms
^- 205.159.239.5                 2  10   377   841   -346us[ -377us] +/-   70ms`
</pre>
