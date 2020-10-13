---
title: PX4 Source code analysis
date: 2018-10-09 19:31:00 +0800
categories: [PX4]
tags: [px4, code]
seo:
  date_modified: 2020-03-25 15:17:00 +0800
---

# Porting work

* Nuttx Config
> /platforms/nuttx/nuttx-configs/xxxx_board

* Board driver
> /Firmware/src/drivers/boards/xxxx_board

* board_config.h  
**BOARD_FMU_GPIO_TAB** is used in fmu.cpp to config basic io(no alternate function yet), so fmu will not override the alternate configuration

* ROMFS

* Nuttx cmake
> /Firmware/cmake/configs/xxxx_board.cmake

* Image
> /platforms/nuttx/Images/xxxx_board.prototype

# Sensors calibration
## Calibration values
* sensors calibration result are stored in param, sensors raw value are scaled and offseted in sensor's driver, the scale and offset parameters are send to drivers using ioctl (e.g. ACCELIOCSSCALE) when parameter updated
* sensors calibration result are stored in param using driver index order. (e.g. d

## Sensor rotation and Calibration
* calibration module subscribe sensor driver output directly(sensor_gyro,sensor_accel,sensor_mag)
* sensor driver's output are rotated(-R) and calibrated
    * driver level's rotate(-R) is used to rotate the sensor to align with board coordinate
    * SENS_BOARD_ROT param is used to rotate board(onboard sensors) to align with vehicle coordinate
    * CAL_MAGx_ROT param is used to rotate external mag sensor to align with vehicle coordinate
* calibration module will set calibration value(offset,scale) to default before calibrate begin, so calibration module can get raw rotated value

### accel calibration module
* accel calibration module need to tell user which vehicle orientation should be hold step by step, so it use sensor_combined(vehicle coordinate sensor value) to check orientation and use SENS_BOARD_ROT param to roate driver output(sensor_accel) to vehicle coordinate
* so, this calibration result need be rotated back using transposed SENS_BOARD_ROT then pass to driver level

## Preflight check
use `CAL_***_PRIME` param to check if primary sensor was calibrated, primary sensor is the sensor orb with highest priority

# Module Base Class
inherit from ```ModuleBase<T>``` class to create a basic module that's already have such as 'start', 'stop' and 'status' commands

# Parameters Definition
## build process
[Params build process]({{ site.baseurl }}{% link _posts/2017-09-29-px4-build-note.md %})

## example
```c
/**
* Roll rate D gain
*
* Roll rate differential gain. Small values help reduce fast oscillations. If value is too big oscillations will appear again.
*
* @min 0.0
* @max 0.01
* @decimal 4
* @increment 0.0005
* @group Multicopter Attitude Control
*/
PARAM_DEFINE_FLOAT(MC_ROLLRATE_D, 0.003f);

/**
* Pitch P gain
*
* Pitch proportional gain, i.e. desired angular speed in rad/s for error 1 rad.
*
* @unit 1/s
* @min 0.0
* @max 10
* @decimal 2
* @increment 0.1
* @group Multicopter Attitude Control
*/
PARAM_DEFINE_FLOAT(MC_PITCH_P, 6.5f);

/**
* Integer bitmask controlling GPS checks.
*
* Set bits to 1 to enable checks. Checks enabled by the following bit positions
* 0 : Minimum required sat count set by EKF2_REQ_NSATS
* 1 : Minimum required GDoP set by EKF2_REQ_GDOP
* 2 : Maximum allowed horizontal position error set by EKF2_REQ_EPH
* 3 : Maximum allowed vertical position error set by EKF2_REQ_EPV
* 4 : Maximum allowed speed error set by EKF2_REQ_SACC
* 5 : Maximum allowed horizontal position rate set by EKF2_REQ_HDRIFT. This check can only be used if the vehicle is stationary during alignment.
* 6 : Maximum allowed vertical position rate set by EKF2_REQ_VDRIFT. This check can only be used if the vehicle is stationary during alignment.
* 7 : Maximum allowed horizontal speed set by EKF2_REQ_HDRIFT. This check can only be used if the vehicle is stationary during alignment.
* 8 : Maximum allowed vertical velocity discrepancy set by EKF2_REQ_VDRIFT
*
* @group EKF2
* @min 0
* @max 511
* @bit 0 Min sat count (EKF2_REQ_NSATS)
* @bit 1 Min GDoP (EKF2_REQ_GDOP)
* @bit 2 Max horizontal position error (EKF2_REQ_EPH)
* @bit 3 Max vertical position error (EKF2_REQ_EPV)
* @bit 4 Max speed error (EKF2_REQ_SACC)
* @bit 5 Max horizontal position rate (EKF2_REQ_HDRIFT)
* @bit 6 Max vertical position rate (EKF2_REQ_VDRIFT)
* @bit 7 Max horizontal speed (EKF2_REQ_HDRIFT)
* @bit 8 Max vertical velocity discrepancy (EKF2_REQ_VDRIFT)
*/
PARAM_DEFINE_INT32(EKF2_GPS_CHECK, 21);
```

# Some confusion
* orb invoke always fail in nsh created thread (like module's print_status())

# orb_copy
* you can pass nullptr to orb_copy to clear subscriber's internal update flag only