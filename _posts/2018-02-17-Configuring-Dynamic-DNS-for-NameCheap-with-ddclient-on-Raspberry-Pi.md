---
title: Configuring Dynamic DNS for NameCheap with ddclient on Raspberry-Pi
published: true
---

I've recently found my name server host (namecheap) supports Dynamic DNS updating of records, so I've added a tool called `ddclient` to my Raspberry Pi

Here is how I have installed and configured this on Raspbian Jessie Lite (Nov 2017)

## Installing `ddclient`

`sudo apt install ddclient libio-socket-ssl-perl`

I'm reasonably sure that the `libio-socket-ssl-perl` will be installed as part of `ddclient` anyway, but just in case this is added too.

When you install `ddclient` you also have to follow through some text/GUI I skipped this, and typed nonsense to get past.

## Creating the config file

Edit the config file as such `sudo nano /etc/ddclient.conf`

<script src="https://gist.github.com/get-fooby/059718fde16f7bf78f7231927052f59a.js"></script>

Each of these lines is specific to NameCheap's config, and we've added `ssl=yes` to ensure the updating is over SSL, as the password NameCheap give can update any record.

* `protocol` in this case is "namecheap"
* `server` is the hostname of the dynamic DNS server. The dynamic DNS servers used by namecheap are "dynamicdns.park-your-domain.com"
* `login` is your domain name
* `password` is the string from the namecheap web interface. Leave the single quotation marks around the string.
* The last line is the `subdomain` to be modified. If you wanted to update your root domain, you would put an `@` symbol on this line instead

## Installing the `ddclient` service

`nano /etc/default/ddclient`

Add in the line `run_daemon=true` and ensure the other two options are set to false.

We also need to ensure this is installed as a service, and restarts at bootup so 

`systemctl enable ddclient.service` will install this as a service on Raspbian, for other O/S check the link [DigitalOcean Linux Services Guide](https://www.digitalocean.com/community/tutorials/how-to-configure-a-linux-service-to-start-automatically-after-a-crash-or-reboot-part-1-practical-examples)

Some content (and a lot more detail) from: [Sam Hobbs](https://samhobbs.co.uk/2015/01/dynamic-dns-ddclient-raspberry-pi-and-ubuntu)
