storagefancontrol
=================

Sets chassis fan speed based on highest disk temperature

Fan control is coverned by the control loop feedback mechanism [PID][pid].

[pid]: http://en.wikipedia.org/wiki/PID_controller  

So if you have 24 drives in a chassis, this script checks the temperature
of each drive and the highest drive temperature is used to determine
if the chassis fans need to run faster, slower or stay at the same speed.

the PID controller makes sure that an optimal fan speed is found to keep the
system at a maximum of - in my case - 40C.

Measurements (temperature and pwm settings) and action output is logged to syslog.
This will give you output on the console:

    export DEBUG=True 

The disk temperature is read through the LSI megacli command-line utility.
If you want to use the values from SMART, this is not the tool for you.
Patches that implement this feature are welcome.

You may want to changes some variables based on your own environment, especially
the /sys/class/hwmon/hwmon2/device/pwm2 value may be different for your systems.
This example is based on the Supermicro X9SCM-F.
