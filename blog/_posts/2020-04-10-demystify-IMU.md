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

* High sampling rate (on the order of hundreds of samples per second)
* Accurate in a short time
* Suffering from drift in a long time range
* Accelerometer and gyroscope can be biased, which means that their measurement errors are not zero-mean (零偏). The bias can vary as time goes. We denote the biases of the accelerometer and gyroscope measurements as $$b^a$$ and $$b^g$$ respectively.

# Model

At time $$t$$,

$$\begin{split} \omega_b(t) &= \bar{\omega}_b(t) + b^g(t) + n^g(t), \\ a_b(t) &= R_{wb}(t)^T (\bar{a}_w(t) - g_w) + b^a(t) + n^a(t). \end{split}$$

* $$\bar{\omega}$$ and $$\bar{a}$$ are the true rotational velocity and acceleration in contrast to the noisy measurements $$\omega$$ and $$a$$.
* $$n^g(t)$$ and $$n^a(t)$$ are Gaussian noise of the gyroscope and acceleration measurements. **Please note that the bias terms and the noise terms are all expressed in the body frame.**
* $$R_{wb}(t)$$ is the rotation which rotates a point **from the body frame into the world frame** at time $$t$$.
* $$g_w$$ is the gravity acceleration vector in the world frame.

# Inertial odometry

Inertial odometry aims at estimating the state of IMU, which is denoted as $$x(t) = [R_{wb}(t), p_w(t), v_w(t)]^T$$. $$p_w(t)$$ and $$v_w(t)$$ are the position and velocity of IMU in the world frame respectively.

$$\begin{split} \frac{d R_{wb}(t)}{dt} &= R_{wb}(t) \frac{d \Delta R_b(t)}{dt} = R_{wb}(t) \bar{\omega}_b(t), \\
\frac{d v_w(t)}{dt} &= \bar{a}_w(t), \\
\frac{d p_w(t)}{dt} &= v_w(t). \end{split}$$

* $$\Delta R_b(t)$$ is the infinitestimal rotation from time $$t + \Delta t$$ to time $$t$$ in the body frame.

The update step of the IMU state can be expressed as

$$\begin{split} R_{wb}(t + \Delta t) &= R_{wb}(t) exp( \bar{\omega}_b(t) \Delta t ) \\ &= R_{wb}(t) exp( (\omega_b(t) - b^g(t) - n^g(t)) \Delta t ), \\
v_w(t + \Delta t) &= v_w(t) + \bar{a}_w(t) \Delta t \\ &= v_w(t) + g_w \Delta t + R_{wb}(t) (a_b(t) - b^a(t) - n^a(t)) \Delta t, \\
p_w(t + \Delta t) &= p_w(t) + v_w(t) \Delta t + \frac{1}{2} \bar{a}_w(t) \Delta t^2 \\ &= p_w(t) + v_w(t) \Delta t + \frac{1}{2} g_w \Delta t^2 + \frac{1}{2} R_{wb}(t) (a_b(t) - b^a(t) - n^a(t)) \Delta t^2. \end{split} $$

* Here we use lie algebra for the representation of rotation velocity.
* Note that we assume a constant rotation $$R_{wb}(t)$$ during the short sampling period $$\Delta t$$.
* Computing $$R_{wb}(t + \Delta t)$$ is straightforward just by intergating the rotational velocity, while computing $$p_w(t + \Delta t)$$ is cumbersome because not only does it rely on the double integration of the acceleration measurements but also the estimation of orientation $$R_{wb}(t)$$.

Equivalently, we can express the equations above in discrete time steps of length $$\Delta t$$. Subscripts are omitted for each of notation.

$$\begin{split} R_{i+1} &= R_i exp( \bar{\omega}_i \Delta t) \\ &= R_i exp( (\omega_i - b^g_i - n^g_i) \Delta t), \\
v_{i+1} &= v_i + \bar{a}_i \Delta t \\ &= v_i + g \Delta t + R_i (a_i - b^a_i - n^a_i) \Delta t, \\
p_{i+1} &= p_i + v_i \Delta t + \frac{1}{2} \bar{a}_i \Delta t^2 \\ &= p_i + v_i \Delta t + \frac{1}{2} g \Delta t^2 + \frac{1}{2} R_i (a_i - b^a_i - n^a_i) \Delta t^2. \end{split} $$

Rather than looking at a discrete step $$(i, i+1)$$, we can pre-integrate the state dynamics in longer intervals, e.g., $$(i, j)$$.

$$\begin{split} R_j &= R_i exp( \sum_{k=i}^{j-1} \bar{\omega}_k \Delta t) \\ &= R_i exp( \sum_{k=i}^{j-1} (\omega_k - b^g_k - n^g_k) \Delta t) \\ &= R_i \prod_{k=i}^{j-1} exp( (\omega_k - b^g_k - n^g_k) \Delta t) \\
v_j &= v_i + \sum_{k=i}^{j-1} \bar{a}_k \Delta t \\ &= v_i + (j-i) g \Delta t + \sum_{k=i}^{j-1} R_k (a_k - b^a_k - n^a_k) \Delta t, \\
p_j &= p_i + \sum_{k=i}^{j-1} v_k \Delta t + \frac{1}{2} \sum_{k=i}^{j-1} \bar{a}_k \Delta t^2 \\ &= p_i + \sum_{k=i}^{j-1} v_k \Delta t + (j-i) \frac{1}{2} g \Delta t^2 + \sum_{k=i}^{j-1} \frac{1}{2} R_k (a_k - b^a_k - n^a_k) \Delta t^2. \end{split} $$


### Pre-integration

The problem of the equations above is that they require the knowledge of the initial state at time $$i$$, i.e., $$x_i = [R_i, p_i, v_i]^T$$. However, in some cases, we have no idea of the initial state. Therefore, pre-integration is usually adopted to integrate the measurements in the body frame of the last state. Then, once the initial state is available, the subsequent states can be brought into the world frame by simple addition or multiplication with the pre-integration values. 

Below, we ignore the addition terms concerning $$g$$ for simplicity, since they are constant for a fixed length of interval. The pre-integration is written as below.

$$\begin{split} \Delta R_{ij} &=   R_i^T R_j = \prod_{k=i}^{j-1} exp( (\omega_k - b^g_k - n^g_k) \Delta t) \\
\Delta v_{ij} &= R_i^T(v_j - v_i) = \sum_{k=i}^{j-1} \Delta R_{ik} (a_k - b^a_k - n^a_k) \Delta t, \\
\Delta p_{ij} &= R_i^T (p_j - p_i - (j-i) v_i )=  \sum_{k=i}^{j-1} \Delta v_{ik} \Delta t +  \sum_{k=i}^{j-1} \frac{1}{2} \Delta R_{ik} (a_k - b^a_k - n^a_k) \Delta t^2. \end{split} $$

* Here $$\Delta R_{ij}$$, $$\Delta v_{ij}$$ and $$\Delta p_{ij}$$ represent the pre-integrated measurements derived from the raw IMU measurements $$a$$ and $$\omega$$, which relate the states of keyframes i and j.
* $$\Delta R_{ij}$$ denotes the **physical** relative rotation that brings a point in the j-th body frame into the i-th body frame.
* But $$\Delta v_{ij}$$ and $$\Delta p_{ij}$$ do not correspond to the true **physical** change in the velocity and position.
* Currently, the right hand side of the above equations are independent of the initial state $$x_i = [R_i, p_i, v_i]^T$$, and can be obtained solely from the measurements.
* Once $$[R_i, p_i, v_i]^T$$ and $$[\Delta R_{ij}, \Delta p_{ij}, \Delta v_{ij}]^T$$ are known, the state $$[R_j, p_j, v_j]^T$$ can be easily computed.

### Noise modelling

The pre-integrated measurements above involve the raw measurement noise. It is better to isolate the noise terms from the pre-integated quantities, so that likelihood models can be easily applied. Therefore, below the noise modeling of the pre-integrated measurements are derived in a way of simple addition or multication with the noise-free terms.

#### Rotation noise

$$\begin{split} \Delta R_{ij} &= \prod_{k=i}^{j-1} exp( (\omega_k - b^g_k) \Delta t - n^g_k \Delta t) \\
&=  \prod_{k=i}^{j-1} exp( (\omega_k - b^g_k) \Delta t) exp( - J_k n^g_k \Delta t) \\
&= \Delta\tilde{R}_{ij} \prod_{k=i}^{j-1} exp (-\tilde{R}_{k+1,j}^T J^k n_k^g \Delta t)  \\
&= \Delta\tilde{R}_{ij}  exp (-\sum_{k=i}^{j-1} \tilde{R}_{k+1,j}^T J^k n_k^g \Delta t) \\
&= \Delta\tilde{R}_{ij} exp(-\delta \Phi_{ij})
 \end{split} 
 $$

 * $$\Delta\tilde{R}_{ij}$$ is the noise-free rotation from frame j to frame i.
 * The rotation noise $$\delta \Phi_{ij} = \sum_{k=i}^{j-1} \tilde{R}_{k+1,j}^T J^k n_k^g \Delta t$$ is essentially the weighted sum of the raw measurement noise $$n_k^g$$. 
 * $$J_k$$ is the **right Jacobian of SO(3)** and $$exp(\Phi + \delta \Phi) \approx exp(\Phi) exp(J(\Phi) \Phi)$$ is the first-order approximation.
 * The third line above is derived based on the formula $$exp(\Phi)R = R exp(R^T \phi)$$, so that $$R_1 exp(\Phi_1) R_2 exp(\Phi_2) = R_1 R_2 exp(R_2^T \Phi_1) exp(\Phi_2)$$. By a series of such operations, we can move the noise terms to the end.

#### Velocity noise

 $$\begin{split}
 \Delta v_{ij} 
 & \approx \sum_{k=i}^{j-1} \Delta \tilde{R}_{ik} (I - \delta \Phi_{ik}^\wedge) (a_k - b^a_k - n^a_k) \Delta t \\
 &= \sum_{k=i}^{j-1} \Delta \tilde{R}_{ik} (a_k - b^a_k) \Delta t - \Delta \tilde{R}_{ik}  \delta \Phi_{ik}^\wedge (a_k - b^a_k) \Delta t - \tilde{R}_{ik} n^a_k \Delta t \\
 &= \Delta \tilde{v}_{ij} + \sum_{k=i}^{j-1} (\Delta \tilde{R}_{ik} (a_k - b^a_k)^\wedge  \delta \Phi_{ik} \Delta t - \tilde{R}_{ik} n^a_k \Delta t) \\
 &= \Delta \tilde{v}_{ij} - \delta v_{ij}
 \end{split}
 $$

 * $$(a_k - b^a_k) \Delta t$$ is the noise-free velocity change in frame k, and $$\Delta \tilde{R}_{ik} (a_k - b^a_k) \Delta t$$ transforms the change into frame i. Therefore, $$\Delta \tilde{v}_{ij} = \sum_{k=i}^{j-1} \Delta \tilde{R}_{ik} (a_k - b^a_k) \Delta t$$ accumutates the velocity changes in later steps in the local frame i.
 * The velocity noise $$\delta v_{ij} = - \sum_{k=i}^{j-1} (\Delta \tilde{R}_{ik} (a_k - b^a_k)^\wedge  \delta \Phi_{ik} \Delta t - \tilde{R}_{ik} n^a_k \Delta t)$$ is concerned with both integrated rotational noise $$\delta \Phi_{ik}$$ and the acceleration noise $$n^a_k$$.
 * The higher-order noise terms such as $$\Phi_{ik} * n^a_k$$ are neglected.

 #### Position noise

$$\begin{split}
\Delta p_{ij} &= 
\sum_{k=i}^{j-1} (\Delta \tilde{v}_{ik} - \delta v_{ik} )\Delta t +  \frac{1}{2} \Delta \tilde{R}_{ik} (I - \delta \Phi_{ik}^\wedge) (a_k - b^a_k) \Delta t^2  - \frac{1}{2} \Delta \tilde{R}_{ik} n_k^a \Delta t^2 \\
&= \sum_{k=i}^{j-1} (\Delta \tilde{v}_{ik} \Delta t + \Delta \tilde{R}_{ik} (a_k - b^a_k) \Delta t^2 ) - \sum_{k=i}^{j-1} (\delta v_{ik} \Delta t + \tilde{R}_{ik} \delta \Phi_{ik}^\wedge (a_k - b^a_k) \Delta t^2 + \frac{1}{2} \Delta \tilde{R}_{ik} n_k^a \Delta t^2)
\end{split}$$

# Reference

[1] [IMU Preintegration on Manifold for Efficient Visual-Inertial Maximum-a-Posteriori Estimation](http://www.roboticsproceedings.org/rss11/p06.pdf) \\ (* Note: there are errors with Eqs. 26.）

[2] [On-Manifold Preintegration for Real-Time Visual-Inertial Odometry](http://rpg.ifi.uzh.ch/docs/TRO16_forster.pdf) \\ (* Note: journal version of [1], recommended.)

[3] [VINS-Mono: A Robust and Versatile Monocular Visual-Inertial State Estimator](https://arxiv.org/pdf/1708.03852.pdf)