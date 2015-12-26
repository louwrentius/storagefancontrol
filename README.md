storagefancontrol 
=================
Fan speed PID controller based on hard drive temperature
--------------------------------------------------------

This script is meant for storage servers with lots of (spinning) hard drives.
It regulates the chassis (PWM) fan speed based on the hard drive temperature. 

The script is intended for people who build large storage servers used in an
environment (at home) where noise matters.

The hard drive temperature is monitored through either SMART or the MegaCLI 
tool used by LSI-based HBA controllers. 

Fan speed is governed by PWM fan controls and sensors as supported by
Linux under /sys/class/hwmon, such as /sys/class/hwmon/hwmon2/device/pwm2.

This script has been updated to handle multiple PWM devices.

Fan control is coverned by the control loop feedback mechanism [PID][pid].
Here is a [nice intro][video01] on PID. By using PID, the script always finds
the optimal fan speed no matter what the circumstances are.

[video01]: https://www.youtube.com/watch?v=UR0hOmjaHp0
[pid]: http://en.wikipedia.org/wiki/PID_controller  

For example, if you have 24 drives in a chassis, this script checks the temperature
of each drive. The temperature of the hottest drive is used to determine if the 
chassis fans need to run faster, slower or if they should stay at the same speed.

the PID controller makes sure that an optimal fan speed is found to keep the
system at a maximum of - in my case - 40C.

The script logs internal variables to syslog by default.

    Temp: 40 | FAN: 51% | PWM: 130 | P=0   | I=51  | D=0   | Err=0  |
    Temp: 40 | FAN: 51% | PWM: 130 | P=0   | I=51  | D=0   | Err=0  |
    Temp: 40 | FAN: 51% | PWM: 130 | P=0   | I=51  | D=0   | Err=0  |
    Temp: 40 | FAN: 51% | PWM: 130 | P=0   | I=51  | D=0   | Err=0  |
    Temp: 39 | FAN: 43% | PWM: 109 | P=-2  | I=50  | D=-5  | Err=-1 |
    Temp: 39 | FAN: 47% | PWM: 119 | P=-2  | I=49  | D=0   | Err=-1 |
    Temp: 40 | FAN: 54% | PWM: 137 | P=0   | I=49  | D=5   | Err=0  |
    Temp: 40 | FAN: 49% | PWM: 124 | P=0   | I=49  | D=0   | Err=0  |
    Temp: 40 | FAN: 49% | PWM: 124 | P=0   | I=49  | D=0   | Err=0  |
    Temp: 40 | FAN: 49% | PWM: 124 | P=0   | I=49  | D=0   | Err=0  |

This will give you output on the console:

    export DEBUG=True 

The disk temperature is read through 'smartctl' (part of smartmontools).
Temperature can also be read through LSI-based HBAs managed by MegaCLI.
You must edit the __main__ function to switch between these two options.

The script performs a poll every 30 seconds by default. 

I'm using this script myself to govern the fan speed of my [storage server][NAS].

NAS: http://louwrentius.com/71-tib-diy-nas-based-on-zfs-on-linux.html

INSTALL
--------
1. Copy the configuration file in /etc/
2. Copy the executable in /usr/sbin or anywhere you want
3. Make sure the script is executed on boot.
