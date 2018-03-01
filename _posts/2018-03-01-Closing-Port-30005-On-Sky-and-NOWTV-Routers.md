---
title: Closing Port 30005 On Sky and NOWTV Routers
published: true
---
Saw this on the web after looking into some IoT research, it turns out that many routers have this port open, with no user interface that allows it to be closed.

Apparently, Sky doesn't use the TR069 protocol, so it shouldn't need to be open. I think more research is needed before pressing ahead, in any case I've copied these instructions from `close-port-30005.weebly.com`.

1. Open your router interface at http://192.168.0.1/
1. Click Security
1. Log in with username: `admin` password: `sky`
1. Click Services > Add Custom Service
    * Name: `TR069`
    * Type: `TCP`
    * Start Port: `30005`
    * Finish Port: `30005`
1. Click Apply
1. Click Firewall Rules
1. Click Add under Inbound Services
    * Service: `TR069`
    * Action: `Block always`
1. Click Apply

Port 30005 is now closed. Firmware updates will still be enabled.
