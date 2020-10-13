---
title: PX4 Debug
date: 2017-09-12 14:13:00 +0800
categories: [PX4]
tags: [px4, debug]
seo:
  date_modified: 2020-03-25 15:21:00 +0800
---

## Debug Print
All these debug prints print to NuttShell uart port
    
- ### printf
C standard print

- ### std::printf
C++ standard print

- ### warnx
the output message will have "WARN" prefix and "[module]" module name

- ### errx
the output message will have "ERROR" prefix and "[module]" module name
**and program will terminate here**

# Hardware debug(Eclipse)
### ~~stlink~~(stlink debug can't work with eclipse now)
[stlink-tools for linux](https://github.com/texane/stlink)

1. use **st-util** to start stlink gdb server (this can be done in eclipse using **External Tools**)
2. use GDB Hardware debugging to debug
