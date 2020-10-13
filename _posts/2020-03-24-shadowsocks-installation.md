---
title: How to install and use shadowsocks on ubuntu
date: 2020-03-24 14:45:00 +0800
categories: [Linux]
tags: [shadowsocks, proxy]
seo:
  date_modified: 2020-03-24 14:45:00 +0800
---

## Shadowsocks installation
```console
sudo apt-get install shadowsocks-libev
```
- config
    ```console
    vim /etc/shadowsocks-libev/config.json
    ```
- autostart
    ```console
    sudo vim /etc/rc.local
    ```
    ```shell
    #!/bin/bash

    ss-local -c /etc/shadowsocks-libev/config.json -d start

    exit 0
    ```


## Proxychain4 for command line proxy
1. build and install proxychain4 from source

    ```console
    git clone git@github.com:rofl0r/proxychains-ng.git
    cd proxychains-ng
    sudo make install
    sudo make install-config
    ```
2. config proxychain4

    ```console
    sudo vim /usr/local/etc/proxychains.conf
    ```
    modify proxy under`[ProxyList]` to `socks5  127.0.0.1 1080`

3. test proxychain4

    ```console
    proxychains4 wget www.google.com
    ```

## SwitchyOmega for Chrome autoproxy

![switchyomega-config1]({{ "/assets/img/sample/switchyomega-config1.png" | relative_url }})
![switchyomega-config2]({{ "/assets/img/sample/switchyomega-config2.png" | relative_url }})
Rule List URL: `https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt`
