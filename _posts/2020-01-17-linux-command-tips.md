---
title: Linux Command Line Tips
date: 2020-01-17 14:45:00 +0800
categories: [Linux]
tags: [command, tips]
seo:
  date_modified: 2020-03-25 15:13:00 +0800
---

# Java version
```shell
tar xzvf openjdk-xxxxx_linux-x64_bin.tar.gz
sudo mv jdk-10 /usr/lib/jvm/xxxxxxxxxxx/
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/xxxxxxxxxxx/bin/java 1
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/xxxxxxxxxxx/bin/javac 1
sudo update-alternatives --config java
```