As part of the bundle of commands that were created in the olden days (not that old) for RTL-SDR there is one interesting one `rtl_tcp` that can be used to bi-directionally to control the SDR dongle and to send the data received over the LAN (cables only please, WiFi can’t handle it)
This guide is my reference, as my Pi gets re-used quite often (Kali, MotionEyeOS, Google AIY Project, Pi Hole…)
This was all done on Raspbian Jessie Lite 10-04-2017.
Guide
First we need to blacklist the device from modprobe loading the kernel module for the defaults.
`pi@raspberrypi:~ $ echo "blacklist dvb_usb_rtl28xxu" | sudo tee /etc/modprobe.d/dvb_usb_rtl28xxu.conf`

Since modprobe loads any file in `/etc/modprobe.d` with the extension of `.conf` we can create a new file by echo‘ing the data to `sudo tee`, as the normal user pi cannot access this folder.
Once this is blacklisted, we can move to download the software we need.
`$ sudo apt-get install rtl_sdr`

After this we can `$ rtl_test -t` and hopefully see output similar to below. I use `rtl_test -t` so it closes itself, and we don’t have to control-c.

This means we have the software loaded, the drivers loaded OK.
Now on to testing the TCP.
First, remind yourself of your Raspberry Pi’s IP address
`$ ifconfig eth0`
Then once you have that, start `rtl_tcp -a <your ip address>`
