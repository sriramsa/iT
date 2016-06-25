Raspberry Pi WiFi Access point
==============================

1. Setup Raspberry Pi
---------------------
#### 1. Boot your Raspberry Pi
Your Raspberry Pi needs.

  - Connect your display. HDMI cable to monitor
  - Keyboard/Mouse 
  - Insert Storage (Micro SD)
  - Operating system to boot:
    - The micro sd card comes with raspbian Linxu OS pre-installed.
  - Connect Power and let it boot.

#### 2. Connect to the network
##### Change hostname from raspberrypi. 
Your default host name for the RPi is 'raspberrypi'. You need to change this before
you connect to the network. This is so that your device does not have conflicts with
other devices in the network. 

##### Connect ethernet cable to RPi
Confirm it is on the network.

##### Update packages installed
Updating will take a while ~40 mins


2. Make your RPi an WiFi Access Point
-------------------------------------
#### 1. Setup wlan0 networking
Assign a static ip address to your wlan0 interface. This interface is where your AP will hosted.

1. Edit /etc/network/interfaces

  ```
    auto wlan0
    iface wlan0 inet static
        address 192.168.42.1
        netmask 255.255.255.0

  ```

2. Get the interface take the new config

  ```
  $ sudo ifdown wlan0
  $ sudo ifup wlan0
  ```

#### 2. Setup hostapd
hostapd is the package you use to make your wireless.

1. Install **hostapd** package

2. Configure hostapd

  - Create **/etc/hostapd/hostapd.conf** to be as below and update your AP_NAME and PASSWORD.

  ```
    interface=wlan0
    driver=nl80211
    ssid=**YOUR_AP_NAME**
    hw_mode=g
    channel=6
    macaddr_acl=0
    auth_algs=1
    ignore_broadcast_ssid=0
    wpa=2
    wpa_passphrase=**YOUR_PASSWORD**
    wpa_key_mgmt=WPA-PSK
    wpa_pairwise=TKIP
    rsn_pairwise=CCMP 
  ```

3. Start hostapd
```
$ hostapd -d /etc/hostapd/hostapd.conf
```
You should see your AP listed.

4. Connect to your AP from your laptop. 

If successfully connected, you should get a valid IP to your machine. If doesn't connect,
see if you have to setup a DHCP server


Appendix:
---------
####A. Logging in

username: **pi**

password: **raspberry**

####B. Setting up a DHCP server

Setup a DHCP server to dish out IPs to machines connecting to it.

1. Install isc-dhcp-server

2. Edit /etc/dhcp/dhcpd.conf

```
subnet 192.168.0.0 netmask 255.255.255.0 {
    range 192.168.42.2 192.168.42.100
    option domain-name-servers 8.8.4.4;
    option routers 192.168.42.1;
    interface wlan0;
}
```

3. Start the dhcp server
```
$ sudo /etc/init.d/isc-dhcp-server restart
```
Hint: If the service refuses to start, look at the logs to see if there is an error in config;
```
$ sudo journalctl -u isc-dhcp-server
```

####C. NAT - Letting clients connected to AP connect to internet 
Configure NAT (Network Address Translation). NAT is a technique that allows several devices to use a single connection to the internet. Linux supports NAT using Netfilter (also known as iptables) and is fairly easy to set up. First, enable IP forwarding in the kernel: 
```
$ sudo sh -c "echo 1 > /proc/sys/net/ipv4/ip_forward"
```

To set this up automatically on boot, edit the file /etc/sysctl.conf and add the following line to the bottom of the file:

```
net.ipv4.ip_forward=1
```

Second, to enable NAT in the kernel, run the following commands:

```
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo iptables -A FORWARD -i eth0 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i wlan0 -o eth0 -j ACCEPT
```

Your Pi is now NAT-ing. To make this permanent so you don't have to run the commands after each reboot, run the following command:

```
$ sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
```

Now edit the file /etc/network/interfaces and add the following line to the bottom of the file:

```
up iptables-restore < /etc/iptables.ipv4.nat
```
####D. Making your services start at bootup
```
$ sudo update-rc.d <service-name> enable
```

If you want to enable hostapd on startup, edit **/etc/default/hostapd.conf**
and add below line to indicate which config the daemon should use
```
DAEMON_CONF="/etc/hostapd/hostapd.conf"
```

