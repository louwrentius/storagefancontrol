storagefancontrol
=================

Sets chassis fan speed based on highest disk temperature

So if you have 24 drives in a chassis, this script checks the temperature
of each drive and the highest drive temperature is used to determine
if the chassis fans need to run faster, slower or stay at the same speed.

Measurements (temperature and pwm settings) and action output is logged to syslog.

The disk temperature is read through the LSI megacli command-line utility.
If you want generic smart output, is possible, but not implemented. 

Temperature changes may imply that a balance between temperature and fan speed is
not reached. Therefore, the temperature will continue to rise beyond the configured
limit if the fan speed is not increased. If the fan speed is changed after the
limit has been crossed, it is too late. As the temperature is increasing towards
the limit, in last degrees before the limit, the script must increase fan speed
as to prevent the temperature to rise beyond the limit. 

The difference in degrees is used as a multiplier for the fan speed increase. So
if the temperature is rising rapidly, the fan speed ajustment will be larger.

You may want to changes some variables based on your own environment, especially
the /sys/class/hwmon/hwmon2/device/pwm2 value may be different for your systems.
