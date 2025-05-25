---
layout: post
title: "Connecting to WPA2 Enterprise on Linux"
categories: [ Linux ]
tags: [ Linux ]
---

# Connecting to WPA2 Enterprise on Linux

I've switched to Pop!_OS as my daily driver since a while back, and now that many that I know are taking a switch too,
an issue a few of them have faced is connecting to the school's WiFi network that uses WPA2 enterprise.

Hopefully, this post will help you with that.

## Background

My school, and many others, use WPA2 Enterprise for their WiFi networks. The issue comes up because they use insecure
renegotiation, relying on TLS 1.0, and OpenSSL (which Linux uses for the handshake) has stopped supporting it a while
back.

## Solution

The easiest solution is to re-enable the insecure renegotiation in OpenSSL. [(Reference)](https://ubuntuforums.org/showthread.php?t=2474436)

1. Create a config file so this insecure negotiation is used for WPA negotiations only
   ```bash
   sudo cp /etc/ssl/openssl.cnf /etc/wpa_supplicant
   ```

2. Enable the insecure renegotiation in the config file
   ```bash
   # Enable legacy TLS
   sudo gedit /etc/wpa_supplicant/openssl.cnf
   ```

3. Do a search for `ssl_conf = ssl_sect` and add the following lines below it:
   ```txt
   [ssl_sect]
   system_default = system_default_sect

   [system_default_sect]
   Options = UnsafeLegacyRenegotiation
   CipherString = DEFAULT@SECLEVEL=1
   ```

4. Next, to make wpa_supplicant use this configuration file:
   ```bash
   sudo gedit /usr/lib/systemd/system/wpa_supplicant.service
   ```

5. Search for `BusName=fi.w1.wpa_supplicant1` and add the following lines below it:
   ```txt
   Environment="OPENSSL_CONF=/etc/wpa_supplicant/openssl.cnf"
   ExecStart=/sbin/wpa_supplicant -u -s -O /run/wpa_supplicant
   ExecReload=/bin/kill -HUP $MAINPID`
   ```

6. Finally, restart the wpa_supplicant service:
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl restart wpa_supplicant.service
   ```

7. In your WiFi settings, ensure that the Authentication is set to `Protected EAP (PEAP)`. If this isn't available in
   your settings, you can also use the following commands to edit the configuration file:
   ```bash
    sudo gedit /etc/NetworkManager/system-connections/<your-network-name>.nmconnection
    ```
   Under the `[802-1x]` section, ensure that it looks something like this:
   ```txt
    [802-1x]
    eap=peap;
    identity=<your-username>
    phase2-auth=mschapv2
    password=<your-password>
   ```
    Save the file and restart the network manager:
    ```bash
     sudo systemctl restart NetworkManager.service
    ```

You should then be able to connect to the network... hopefully... 🤞
