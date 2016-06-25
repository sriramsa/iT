Raspberry Pi WiFi Access point
==============================

Setup Raspberry Pi
---------------------
### Boot your Raspberry Pi
Your Raspberry Pi needs.
1. Display
  - HDMI cable to monitor
2. Keyboard/Mouse 
3. Insert Storage (Micro SD)
4. Operating system to boot:
  - The micro sd card comes with raspbian Linxu OS pre-installed.
5. Connect Power and let it boot.

### Connect to the network
#### Change hostname from raspberrypi. 
Your default host name for the RPi is 'raspberrypi'. You need to change this before
you connect to the network. This is so that your device does not have conflicts with
other devices in the network. 


edit /etc/hostname
then run
/etc/init.d/hostname.sh script to make it live.
or
reboot

Connect ethernet cable to RPi
Confirm it is on the network.

Update packages installed.

Appendix:
Raspberry standard user: pi
password: raspberry


Make your RPi an WiFi Access Point:
-----------------------------------
Setup a DHCP server to dish out IPs to machines connecting to it.
http://itsacleanmachine.blogspot.com/2013/02/wifi-access-point-with-raspberry-pi.html

Setup WiFi access point:
update package list
install hostapd

