I’ve reloaded my Raspberry Pi 2 with Raspbian Jessie to act as a RTL_SDR server (post soon!) but found that wiping the image of course loses my WiFi settings.

Here is how to reset these – you will need a screen and keyboard. Thankfully the Raspberry Pi uses HDMI and can use a LCD TV if needed.

`$ sudo nano /etc/network/interfaces`

Once in here edit the lines to add your network – now this looks to me that this will only apply to that network adapter – and not all (by use of wpa_supplicant.conf) but we will leave that for another day.

```
GNU nano 2.2.6         File: /etc/network/interfaces
# interfaces(5) file used by ifup(8) and ifdown(8)
# Please note that this file is written to be used with dhcpcd
# For static IP, consult /etc/dhcpcd.conf and 'man dhcpcd.conf'</code>

# Include files from /etc/network/interfaces.d:
source-directory /etc/network/interfaces.d

auto lo
iface lo inet loopback

iface eth0 inet manual

allow-hotplug wlan0
iface wlan0 inet dhcp
wpa-ssid "yourssidhere"
wpa-psk "yourpasswordhere"

allow-hotplug wlan1
[ Read 21 lines ]
^G Get Help ^O WriteOut ^R Read File ^Y Prev Page ^K Cut Text ^C Cur Pos
^X Exit ^J Justify ^W Where Is ^V Next Page ^U UnCut Text^T To Spell
```

Once this has applied, reboot the pi

`$ sudo reboot`
