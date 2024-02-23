# upboard_setup

## pinctrl-upboard
UP boards pin controller driver for UP HAT pins.

Install deb package
=============================================
Install deb package on Debian-based Linux distributions like Ubuntu, Linux Mint, Parrot....

1. install DKMS
---------------
```
sudo apt install dkms 
```
Reboot the system before installing the pinctrl driver.
Download the latest deb package from [the release folder](https://github.com/up-division/pinctrl-upboard/releases)

2. install deb package
------------------------
```
sudo dpkg -i pinctrl-upboard_1.1.3_all.deb
```
Reboot the system again before starting to use the 40 pin header functionalities.

HAT Pins usage information
==========================================================================================
please refer [UP WiKi for more details](https://github.com/up-board/up-community/wiki/40Pin-Header)
==========================================================================================
------------------------
------------------------
## GPIO/I2C/SPI-access without root-permissions
If you do not wish to use the script, you can set things up manually as follows:
### SPI-bus access
For SPI-bus access create the file /etc/udev/rules.d/50-spi.rules with the following contents:
```
sudo nano /etc/udev/rules.d/50-spi.rules
```
```
SUBSYSTEM=="spidev", GROUP="spiuser", MODE="0660"
```
Next, copy and paste or type these lines into the terminal as the user you want to give access to the bus:
```
sudo groupadd spiuser
sudo adduser "$USER" spiuser
```
Note : Please change Pin16 to output in the BIOS/HAT Configuration, and the SPI CLK will be normal.
### I2C-bus access
For I2C-bus access create the file /etc/udev/rules.d/50-i2c.rules with the following contents:
```
sudo nano /etc/udev/rules.d/50-i2c.rules
```
```
SUBSYSTEM=="i2c-dev", GROUP="i2cuser", MODE="0660"
```
Similar to above, copy and paste or type these lines into the terminal as the user you want to give access to the bus:
```
sudo groupadd i2cuser
sudo adduser "$USER" i2cuser
```
### GPIO-access
For GPIO access create the file /etc/udev/rules.d/50-gpio.rules with the following contents:
```
sudo nano /etc/udev/rules.d/50-gpio.rules
```
```
SUBSYSTEM=="gpio*", PROGRAM="/bin/sh -c '\
        chown -R root:gpiouser /sys/class/gpio && chmod -R 770 /sys/class/gpio;\
        chown -R root:gpiouser /sys/devices/virtual/gpio && chmod -R 770 /sys/devices/virtual/gpio;\
        chown -R root:gpiouser /sys$devpath && chmod -R 770 /sys$devpath\
'"
```
Again, copy and paste or type these lines into the terminal as the user you want to give access to the bus:
```
sudo groupadd gpiouser
sudo adduser "$USER" gpiouser
```
Now, there is nothing else to do with the SPI-bus or I2C-bus other than to reboot.
