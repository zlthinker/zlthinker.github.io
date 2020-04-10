---
title: Demystify Innertial Measurement Unit (IMU)
updated: 2020-04-10 14:00
---

* TOC
{:toc}

# Components

|Components| Measurements| DoF  | Description |
|:--------:|:-----------:|:----:|:------------:|
| accelerometer | $$a_b = [a_x, a_y, a_z]^T$$ | 3 | acceleration (togerher with gravity) in x-, y-, z-axes|
| gyroscope | $$\omega_b = [\omega_x, \omega_y, \omega_z]^T$$ | 3 | angular velocity around x-, y-, z-axes|

**Remark** The subscript $$*_b$$ means that the measurements are expressed in the body frame of IMU.

# Properties

* High sampling rate
* Accurate in a short time
* Suffering from drift in a long time range
* Accelerometer and gyroscope can be biased, which means that their measurement errors are not zero-mean. The bias can vary as time goes.

# Calibration

# Application 

### Localization