storagefancontrol
=================

Sets chassis fan speed based on highest disk temperature

So if you have 24 drives in a chassis, this script checks the temperature
of each drive and the highest drive temperature is used to determine
if the chassis fans need to run faster, slower or stay at the same speed.

Measurements (temperature and pwm settings) and action output is logged to syslog.

The disk temperature is read through the LSI megacli command-line utility.
If you want to use the values from SMART, this is not the tool for you.
Patches that implement this feature are welcome.

Temperature changes may imply that a balance between temperature and fan speed is
not reached. These changes, even within reasonable temperature ranges, can be 
used to anticipate and react before the temperature threshold is actually
surpassed. 

The difference in degrees between the temperature threshold and the actual temperature
is used as a multiplier for the fan speed ajustment. So if the temperature is rising 
or falling rapidly, the fan speed ajustment will be larger.

You may want to changes some variables based on your own environment, especially
the /sys/class/hwmon/hwmon2/device/pwm2 value may be different for your systems.
This example is based on the Supermicro X9SCM-F.
