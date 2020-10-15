---
title: px4 simulation
date: 2020-05-11 13:00:00 +0800
categories: [PX4]
tags: [px4, sitl, hil]
seo:
  date_modified: 2020-10-15 15:03:00 +0800
---
# Simulation architecture

## 1.SITL

![sitl-arch]({{ "/assets/img/sample/px4_sitl_overview.png" | relative_url }})

SITL simulator runs as a TCP server(local port: 4560), px4 sitl runs as a TCP client to connect to simulator, px4 sitl also open a UDP connection(local port: 14570) for QGC(local port: 14550)

## 2.HITL

![hitl-arch]({{ "/assets/img/sample/px4_hitl_overview.png" | relative_url }})

HITL simulator connect to px4 hardware using usb-serial port(e.g. /dev/ttyACM0 in Linux), and route mavlink messages to UDP for QGC

# mavlink-router
[mavlink-router github page](https://github.com/intel/mavlink-router)

## configuration
- UdpEndpoint
	- Mode = Eavesdropping
		mavlink-router will listen for incoming packets from
	- Mode = Normal
		mavlink-router will route messages to (and from)
- TcpEndpoint
	remote TCP server
- TcpServerPort
	simulated local TCP server

# develop note
## vmware gazebo execute fault
if enable ***3D acceleration*** in vmware for ubuntu, gazebo will throw fault `VMware: vmw_ioctl_command error Invalid argument`
	solved it with the following environmental variable presumably shrinking down the graphics instruction set to OpenGL2
```shell
export SVGA_VGPU10=0
```

## HAS_GYRO TRUE fault
You can fix it manually by opening ***Firmware/Tools/sitl_gazebo/include/gazebo_opticalflow_plugin.h*** and replacing 
```C
#define HAS_GYRO TRUE
```
 with
```C
#define HAS_GYRO true
```
(make TRUE lowercase).
It seems like the submodule related to the tag v1.10.1 does not have the update that fixes this

## gazebo models download
[example1](http://machineawakening.blogspot.com/2015/05/how-to-download-all-gazebo-models.html)
[example2](https://stackoverflow.com/questions/37105913/connecting-to-gazebo-model-database)