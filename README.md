# Artisan-tmd-56 Linux SetUp
## High Level Overview
The TMD-56 device needs to be recognized by the linux machine through the CP210x driver. Once detected, the USB path needs to be specified in the comm port configuration. Once this mapping is done, we need to ensure that the path is writeable (by changing mode to 666).

## Prerequisites / KT (Knowledge Transfer)
#### CP210x driver
The CP210x driver handles USB port recognition. By default linux kernel comes pre packaged with it, does not have to be installed manually. If your computer can read USB ports, the driver is installed.

## SetUp Steps
The following are commands to be run in terminal
1. lsusb\
_To check usb port recognition look for "Bus 008 Device 008: ID 0403:6001 Future Technology Devices International, Ltd FT232 Serial (UART) IC" - if it is present, driver is working and device is detected_
2. dmesg\
_run dmesg right after plugging in the TMD-56 look for "**FTDI USB Serial Device converter detected**" followed by: **FTDI USB Serial Device converter now attached to [ttyUSB0]**(whichever USB port specified needs to be saved for later)_
3. write down / copy the following: /dev/[usb port] | ex: /dev/ttyUSB0
4. Go to artisan and select\
_  config -> devices -> ok -> Copy paste the previous path in the comm port -> ok_
5. Select START or ON
_if successful, readings should be 0 and status on the top left should say monitoring...if unsuccessful proceed to the next step "Giving write access to TMD-56"._

#### Giving write access to TMD-56
If artisan is stating "cannot open serial port" on the left as an error message, do the following in terminal
1. cd /etc/udev/rules.d
2. ls
_identify "99-artisan.rules" file in the output of ls command_
3. nano 99-artisan.rules

nano opens a file for read/write. on a new line, paste the following: \
**SUBSYSTEMS=="usb", ACTION=="add", ATTRS{idVendor}=="<REPLACE_ME>", ATTRS{idProduct}=="<REPLACE_ME>", MODE="666"**

replace idVendor and idProduct value with the vendor and product id of the TMD-56. You can find the idVendor and idProduct using the dmesg command provided earlier and looking for vendor id and product id. It will look like the following:

[ 2764.529118] ftdi_sio ttyUSB0: FTDI USB Serial Device converter now disconnected from ttyUSB0\
[ 2764.529163] ftdi_sio 8-1:1.0: device disconnected\
[ 2766.500064] usb 8-1: new full-speed USB device number 8 using xhci_hcd\
[ 2766.701465] usb 8-1: New USB device found, idVendor=0403, idProduct=6001, bcdDevice= 6.00

make sure to identify the ids for **FTDI USB SERIAL DEVICE CONVERTER**

After doing the above steps, re plug in the TMD-56 and start a session, the port should be detected and state should be monitoring with temperatures as 0 initially (not half zeroes)

