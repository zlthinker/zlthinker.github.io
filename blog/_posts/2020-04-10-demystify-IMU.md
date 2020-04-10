---
title: Demystify Innertial Measurement Unit (IMU)
updated: 2020-04-10 14:00
---

* TOC
{:toc}

# Components

|Components| Measurements| DoF  | Description |
|:--------:|:-----------:|:----:|:------------:|
| accelerometer | $$a_b = [a_{bx}, a_{by}, a_{bz}]^T$$ | 3 | acceleration (togerher with gravity) in x-, y-, z-axes|
| gyroscope | $$\omega_b = [\omega_{bx}, \omega_{by}, \omega_{bz}]^T$$ | 3 | angular velocity around x-, y-, z-axes|

**Remark** The subscript $$*_b$$ means that the measurements are expressed in the body frame of IMU.

# Properties

* High sampling rate
* Accurate in a short time
* Suffering from drift in a long time range
* Accelerometer and gyroscope can be biased, which means that their measurement errors are not zero-mean. The bias can vary as time goes. We denote the biases of the accelerometer and gyroscope measurements as $$b^a$$ and $$b^g$$ respectively.

# Model

At time $$t$$,

$$\omega_b(t) = \bar{\omega}_b(t) + b^g(t) + n^g(t),$$

$$a_b(t) = R_{wb}(t)^T (\bar{a}_w(t) - g_w) + b^a(t) + n^a(t).$$

* $$\bar{\omega}$$ and $$\bar{a}$$ are the true rotational velocity and acceleration in contrast to the noisy measurements $$\omega$$ and $$a$$.
* $$n^g(t)$$ and $$n^a(t)$$ are Gaussian noise of the gyroscope and acceleration measurements.
* $$R_{wb}(t)$$ is the rotation which rotates a point from the body frame into the world frame at time $$t$$.
* $$g_w$$ is the gravity acceleration vector in the world frame.

# Application 

### Inertial odometry

Inertial odometry aims at estimating the state of IMU, which is denoted as $$x(t) = [R_{wb}(t), p_w(t), v_w(t)]^T$$. $$p_w(t)$$ and $$v_w(t)$$ are the position and velocity of IMU in the world frame.

$$\frac{d R_{wb}(t)}{dt} = R_{wb}(t) \frac{d \Delta R_b(t)}{dt} = R_{wb}(t) \omega_b(t),$$

$$\frac{d v_w(t)}{dt} = \bar{a}_w(t),$$

$$\frac{d p_w(t)}{dt} = v_w(t).$$

* $$\Delta R_b(t)$$ is the infinitestimal rotation in the body frame.
* $$p_w(t)$$ is the position of IMU in the world frame.