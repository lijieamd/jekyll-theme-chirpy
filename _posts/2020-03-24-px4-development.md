---
title: PX4 Development
date: 2020-03-24 15:32:00 +0800
categories: [PX4]
tags: [px4, development]
seo:
  date_modified: 2020-03-24 15:32:00 +0800
---
# Eclipse plugins
- [GNU MCU Eclipse plug-ins](https://gnu-mcu-eclipse.github.io/)
- DevStyle
- CMake Editor
- LiClipseText
- TM Terminal

# Eclipse settings
## Verbose build
- bootloader
![flameshot-shortcut]({{ "/assets/img/sample/verbose-build1.png" | relative_url }})
- firmware
![flameshot-shortcut]({{ "/assets/img/sample/verbose-build2.png" | relative_url }})

## Preprocessor settings for code indexer
`${COMMAND} ${COMPILER_FLAGS} -E -P -v -dD "${INPUTS}"`  
Add a Environment variable(search makefile for platform specific flags):  
`COMPILER_FLAGS=-g -mthumb -mcpu=cortex-m7 -mfloat-abi=hard -mfpu=fpv5-sp-d16`
![flameshot-shortcut]({{ "/assets/img/sample/eclipse-preprocessor-setting1.png" | relative_url }})
`(.*gcc)|(.*[gc]\+\+)|(.*clang)`
![flameshot-shortcut]({{ "/assets/img/sample/eclipse-preprocessor-setting2.png" | relative_url }})

## SITL source debug setting
- GDB debug
![flameshot-shortcut]({{ "/assets/img/sample/sitl-debug1.png" | relative_url }})
![flameshot-shortcut]({{ "/assets/img/sample/sitl-debug2.png" | relative_url }})
![flameshot-shortcut]({{ "/assets/img/sample/sitl-debug3.png" | relative_url }})
- Jmavsim for SITL
![flameshot-shortcut]({{ "/assets/img/sample/sitl-jmavsim1.png" | relative_url }})
![flameshot-shortcut]({{ "/assets/img/sample/sitl-jmavsim2.png" | relative_url }})
- Jmavsim for HIL
![flameshot-shortcut]({{ "/assets/img/sample/hil-jmavsim.png" | relative_url }})

# JLink
- [Load binary file using jlink command](https://wiki.segger.com/J-Link_Commander)  
Create JLink CommandFile:  
```shell
si swd
speed 4000
device STM32F767II
r
h
loadbin ../Bootloader/build/px4fmuv5_bl/px4fmuv5_bl.bin 0x08000000
q
```
Use like this:  
`JLinkExe -CommandFile filecreated`

# Log
- [FlightReview Source Code](https://github.com/PX4/flight_review)
- [FlightReview Official Web Service](https://review.px4.io/)
- [Python tool for parse ulog file](https://github.com/PX4/pyulog)
