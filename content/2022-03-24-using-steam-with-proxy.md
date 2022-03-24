+++
title = "Using a Proxy with Steam on Linux"
description = ""
date = "2022-10-25"

[taxonomies]
tags = ["linux", "steam"]
categories = ["tutorial"]
+++

_Using a proxy is against the Steam terms of service. Proceed at your own risk._

One of the limitations Steam has is the lack of support for proxies. Especially if you are working in an environment behind a proxy this becomes really annoying. 

However, in Linux systems, it's really easy to get the Steam client working under a proxy. The following steps will explain how to get this working.

#### Step 1 
Figure out where Steam binary is installed in your machine. This can be done with the help of ``whereis`` command. In my system, it's installed under ``/user/games``
```bash
[si@vet ~]$ whereis steam
steam: /usr/games/steam /usr/share/man/man6/steam.6.gz
```

#### Step 2
Start Steam with ``http_proxy`` environment variable set in your terminal. In my system, the proxy is running in localhost port 443. 

```bash
[si@vet ~]$ env http_proxy=http://127.0.0.1:443/ /usr/bin/steam -tcp %U
```
This will make Steam use the ``http_proxy`` you specified.

That's pretty much it. If you monitor your proxy you will notice all Steam requests are being forwarded through that. 

#### Step 3
You can go one step further and make your life a little bit easier by creating a custom ``.desktop`` file in your ``~/.local/share/application`` to start Steam with the proxy.

```bash
[Desktop Entry]
Name=Steam Proxied
Exec=env http_proxy=http://127.0.0.1:443/ /usr/bin/steam -tcp %U
Type=Application
NoDisplay=true
Categories=Games;
```
By creating a separate ``.desktop`` file, you will be able to start the Steam client with or without a proxy very conveniently.