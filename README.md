storagefancontrol
=================

Sets chassis fan speed based on highest disk temperature

So if you have 24 drives in a chassis, this script checks the temperature
of each drive and the highest drive temperature is used to determine
if the chassis fans need to run faster, slower or stay at the same speed.

Output is logged to syslog.

The disk temperature is read through the LSI megacli command-line utility.
If you want generic smart output, is possible, but not implemented. 

You may want to changes some variables based on your own environment, especially
the /sys/class/hwmon/hwmon2/device/pwm2 value may be different for your systems.
