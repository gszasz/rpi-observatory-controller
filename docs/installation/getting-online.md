# Getting Online

Note that Raspbian does not have
[NetworkManager](https://wiki.debian.org/NetworkManager) installed to setup your
network.  You need to setup network manually in old-school Debian way.


## Setting up wireless connection

For testing and compilation purposes it can be useful to temporarily connect
Raspberry Pi box to local wi-fi network.

1. Disable wireless interface.

        $ sudo ifdown wlan0

2. Add new connection to `wpa_supplicant.conf`.

        $ sudo nano /etc/wpa_supplicant/wpa_supplicant.conf

   Add following lines (replace `SSID` and `passphrase` with actual SSID and
   passphrase for your network).

        network={
            ssid="SSID"
            psk="passphrase"
            key_mgmt=WPA-PSK
        }

3. Bring wireless interface back online.

        $ sudo ifup wlan0

Note that wireless connection should be used only for testing and development.


## Permanently disable wireless connection

Since observatory controller should be connected to LAN via wired connection, it
is good idea to disable wireless interface and permanently unload wireless
adapter kernel modules.

1. Disable wireless interface.

        $ sudo ifdown wlan0

2. Remove `wlan0` interface from the network configuration.

        $ sudo nano /etc/network/interfaces

   Comment out following lines:

        # allow-hotplug wlan0
        # iface wlan0 inet manual
        #     wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf

3. Then we can permanently turn off the wireless adapter:

        $ sudo nano /boot/config.txt

    Add following line to the file:

        dtoverlay=pi3-disable-wifi

4. Reboot Raspberry Pi

        $ sudo reboot


## Setting wired connection via DHCP

If your router is assigning your Raspberry Pi a stable IP based on MAC address,
setup of your Ethernet interface is rather simple.

1. Disable ethernet interface.

        $ ifdown eth0

2. Configure `eth0` interface to use DHCP.

        $ sudo nano /etc/network/interfaces

   Modify lines related to `eth0` to the following.

        auto eth0
        allow-hotplug eth0
        iface eth0 inet dhcp

3. Enable ethernet interface.

        $ sudo ifup eth0


## Setting static wired connection

1. Disable ethernet interface.

        $ ifdown eth0

2. Configure static interface parameters

        $ sudo nano /etc/network/interfaces

   Modify lines related to `eth0` to the following.  Note that actual IP
   addresses depends on your network.

        auto eth0
        iface eth0 inet dhcp
            address 192.168.1.108
            netmask 255.255.255.0
            gateway 192.168.1.1
            dns-nameservers 12.34.56.78 12.34.56.79

3. Enable ethernet interface.

        $ sudo ifup eth0


---

[Home](../README.md) > [Installation](README.md) > *Getting Online*
