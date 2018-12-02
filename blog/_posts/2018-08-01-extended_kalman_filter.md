---
title: Extended Kalman Filter (EKF)
updated: 2018-08-01 14:00
---


* TOC
{:toc}

## Related Works

[Long Short-Term Memory Kalman Filters: Recurrent Neural Estimators for Pose Regularization](http://openaccess.thecvf.com/content_ICCV_2017/papers/Coskun_Long_Short-Term_Memory_ICCV_2017_paper.pdf)

## Derivation
The calculus of the main component of EKF can be expressed as

$$\begin{split} \mathbf{x}_k &= \mathbf{f}( \mathbf{x}_{k-1}) + \mathbf{w}_{k-1} , \,\,\,\,(1) \\ \mathbf{z}_k &= \mathbf{h}( \mathbf{x}_{k}) + \mathbf{v}_{k}, \,\,\,\,(2) \end{split}$$

where

|Variable| Dim | Description |
|:--:|:--:|:--:|
|$$\mathbf{x}_k$$| $$n \times 1$$| State vector at time $$k$$|
|$$\mathbf{w}_k$$| $$n \times 1$$| Zero-mean Gaussian process noise vector |
|$$\mathbf{z}_k$$| $$m \times 1$$| Observation vector |
|$$\mathbf{v}_k$$| $$m \times 1$$| Zero-mean Gaussian measurement noise vector |
|$$\mathbf{f}(.)$$| $$n \times 1$$| Process function |
|$$\mathbf{h}(.)$$| $$m \times 1$$| Observation function |
|$$\mathbf{Q}_k=E(\mathbf{w}_k \mathbf{w}_k^T)$$| $$n \times n$$| Process noise covariance matrix |
|$$\mathbf{R}_k=E(\mathbf{v}_k \mathbf{v}_k^T)$$| $$m \times m$$| Measurement noise covariance matrix |

In a dynamic process, the initial state $$\mathbf{x}_0$$ is known with a mean $$\mathbf{\mu}_0^a$$ and a covariance $$\mathbf{P}_0^a$$. If the process function $$\mathbf{f}(.)$$ and the observation function $$\mathbf{h}(.)$$ are linear, the Extended Kalman Filter will be equivalent to Kalman Filter and all the subsequent states are gaussian distributed. Most often, the two functions are nonlinear, so that the Extended Kalman Filter approximates them linearly by Taylor Expansion.


In the new time step $$k$$, two pieces of information are available.

(a) The first one can be obtained by Eq. (1) where current state $$\mathbf{x}_k^f$$ with mean $$\mu_k^f$$ and covariance $$\mathbf{P}_k^f$$ is inferred from last state $$\mathbf{x}_{k-1}$$ with mean $$\mu_{k-1}^a$$ and covariance $$\mathbf{P}_{k-1}^a$$. The taylor expansion of $$\mathbf{f}(\mathbf{x})$$ around $$\mu_{k-1}^a$$ gives

$$\begin{split} \mathbf{f}(\mathbf{x}_{k-1}) & \approx \mathbf{f}(\mathbf{\mu}_{k-1}^a) + \mathbf{J}_{\mathbf{f}}(\mathbf{\mu}_{k-1}^a) (\mathbf{x}_{k-1} - \mathbf{\mu}_{k-1}^a) \\ &= \mathbf{f}(\mathbf{\mu}_{k-1}^a) + \mathbf{J}_{\mathbf{f}}(\mathbf{\mu}_{k-1}^a) \mathbf{e}_{k-1}^a, \end{split}$$

where $$\mathbf{J}_{\mathbf{f}}(.)$$ is the Jacobian function and $$\mathbf{e}_{k-1}^a = \mathbf{x}_{k-1} - \mathbf{\mu}_{k-1}^a$$.

$$\begin{split} \mu_k^f &= E(\mathbf{f}(\mathbf{x}_{k-1}) + \mathbf{w}_{k-1}) \\ &=  E(\mathbf{f}(\mathbf{x}_{k-1})) \\ &= E(\mathbf{f}(\mathbf{\mu}_{k-1}^a) + \mathbf{J}_{\mathbf{f}}(\mathbf{\mu}_{k-1}^a) \mathbf{e}_{k-1}) \\ &= \mathbf{f}(\mathbf{\mu}_{k-1}^a) + \mathbf{J}_{\mathbf{f}}(\mathbf{\mu}_{k-1}^a) E(\mathbf{e}_{k-1}) \\ &= \mathbf{f}(\mathbf{\mu}_{k-1}^a). \end{split}$$

The error of estimate $$\mathbf{x}_{k}$$ by $$\mu_k^f$$ is:

$$\begin{split} \mathbf{e}_k^f & = \mathbf{x}_{k} - \mathbf{\mu}_k^f \\ &= \mathbf{f}(\mathbf{x}_{k-1}) + \mathbf{w}_{k-1} - \mathbf{f}(\mathbf{\mu}_{k-1}^a) \\ & \approx \mathbf{J}_{\mathbf{f}}(\mathbf{\mu}_{k-1}^a) \mathbf{e}_{k-1}^a + \mathbf{w}_{k-1} \end{split}$$

$$\begin{split} \mathbf{P}_k^f &= E(\mathbf{e}_k^f {\mathbf{e}_k^f}^T) \\ &= \mathbf{J}_{\mathbf{f}} E(\mathbf{e}_{k-1}^a {\mathbf{e}_{k-1}^a}^T) \mathbf{J}_{\mathbf{f}}^T + E(\mathbf{w}_{k-1} \mathbf{w}_{k-1}^T) \\ &= \mathbf{J}_{\mathbf{f}} \mathbf{P}_{k-1}^a \mathbf{J}_{\mathbf{f}}^T + \mathbf{Q}_{k-1}. \end{split}$$

Overall, the last state yield the gaussian estimation of current state: $$mean = \mu_k^f = \mathbf{f}(\mu_{k-1}^a)$$, $$covariance = \mathbf{P}_k^f$$. The covariance of the estimation is the sum of the covariance derived from last estimate and the covariance of process noise.

(b) The second one is obtained by Eq. (2) based on measurements from current observation $$\mathbf{z}_k$$ with covariance $$\mathbf{R}_k$$.

One way to combine the two pieces of information is to assume the estimation as a linear combination of both $$\mathbf{x}_k^f$$ and $$\mathbf{z}_k$$. Let

$$\mu_k^a = \mathbf{a} + \mathbf{K}_k \mathbf{z}_k.$$

From the unbiasedness condition,

$$\begin{split} E(x_k - \mu_k^a) &= E( \mu_k^f + e_k^f - (\mathbf{a} + \mathbf{K}_k \mathbf{h}(\mathbf{x}_k) + \mathbf{K}_k \mathbf{v}_k)) \\ & = \mu_k^f - \mathbf{a} - \mathbf{K}_k E(\mathbf{h}(\mathbf{x}_k)) \\ & = 0. \end{split}$$

$$\mathbf{a} = \mu_k^f - \mathbf{K}_k E(\mathbf{h}(\mathbf{x}_k)).$$

$$\begin{split} \mu_k^a &= \mu_k^f - \mathbf{K}_k E(\mathbf{h}(\mathbf{x}_k)) + \mathbf{K}_k \mathbf{z}_k \\ &= \mu_k^f + \mathbf{K}_k (\mathbf{z}_k - E(\mathbf{h}(\mathbf{x}_k))). \end{split}$$

Similarly, expand $$\mathbf{h}(\mathbf{x}_k)$$ in Taylor series around $$\mu_k^f$$,

$$\mathbf{h}(\mathbf{x}_k) \approx \mathbf{h}(\mu_k^f) + \mathbf{J}_h(\mu_k^f) (\mathbf{x}_k - \mu_k^f) = \mathbf{h}(\mu_k^f) + \mathbf{J}_h(\mu_k^f) \mathbf{e}_k^f.$$

$$ E(\mathbf{h}(\mathbf{x}_k)) = \mathbf{h}(\mu_k^f) + \mathbf{J}_h E(e_k^f)  = \mathbf{h}(\mu_k^f).$$

$$\mu_k^a =  \mu_k^f + \mathbf{K}_k (\mathbf{z}_k - \mathbf{h}(\mu_k^f) )$$

The error of estimate $$\mathbf{x}_{k}$$ by $$\mu_k^a$$ is:

$$\begin{split} \mathbf{e}_k^a &= \mathbf{x}_{k} - \mu_k^a \\ &= \mathbf{f}(\mathbf{x}_{k-1}) + \mathbf{w}_{k-1} - \mu_k^f - \mathbf{K}_k (\mathbf{z}_k - \mathbf{h}(\mu_k^f) )  \\ &= \mathbf{f}(\mathbf{x}_{k-1}) - \mathbf{f}(\mu_{k-1}^a) + \mathbf{w}_{k-1} - \mathbf{K}_k ( \mathbf{h}( \mathbf{x}_{k}) - \mathbf{h}(\mu_k^f) + \mathbf{v}_{k} ) \\ &= \mathbf{J}_{\mathbf{f}}(\mathbf{\mu}_{k-1}^a) \mathbf{e}_{k-1}^a + \mathbf{w}_{k-1} - \mathbf{K}_k ( \mathbf{J}_{\mathbf{h}}(\mathbf{\mu}_{k}^f) \mathbf{e}_k^f +  \mathbf{v}_{k}) \\ &=  \mathbf{J}_{\mathbf{f}}(\mathbf{\mu}_{k-1}^a) \mathbf{e}_{k-1}^a + \mathbf{w}_{k-1} - \mathbf{K}_k \mathbf{J}_{\mathbf{h}}(\mathbf{\mu}_{k}^f) \mathbf{e}_k^f - \mathbf{K}_k \mathbf{v}_{k} \\ &=  \mathbf{J}_{\mathbf{f}}(\mathbf{\mu}_{k-1}^a) \mathbf{e}_{k-1}^a + \mathbf{w}_{k-1} - \mathbf{K}_k \mathbf{J}_{\mathbf{h}}(\mathbf{\mu}_{k}^f) (\mathbf{J}_{\mathbf{f}}(\mathbf{\mu}_{k-1}^a) \mathbf{e}_{k-1}^a + \mathbf{w}_{k-1}) - \mathbf{K}_k \mathbf{v}_{k} \\ &= (\mathbf{I} - \mathbf{K}_k \mathbf{J}_{\mathbf{h}}(\mathbf{\mu}_{k}^f)) \mathbf{J}_{\mathbf{f}}(\mathbf{\mu}_{k-1}^a) \mathbf{e}_{k-1}^a + (\mathbf{I} - \mathbf{K}_k \mathbf{J}_{\mathbf{h}}(\mathbf{\mu}_{k}^f)) \mathbf{w}_{k-1} - \mathbf{K}_k \mathbf{v}_{k} \end{split}.$$

The covariance of the estimate is

$$\begin{split} \mathbf{P}_k^a &= E(\mathbf{e}^k {\mathbf{e}^k}^T) \\ &= (\mathbf{I} - \mathbf{K}_k \mathbf{J}_{\mathbf{h}}(\mathbf{\mu}_{k}^f)) \mathbf{J}_{\mathbf{f}}(\mathbf{\mu}_{k-1}^a) \mathbf{P}_{k-1}^a \mathbf{J}_{\mathbf{f}}(\mathbf{\mu}_{k-1}^a)^T (\mathbf{I} - \mathbf{K}_k \mathbf{J}_{\mathbf{h}}(\mathbf{\mu}_{k}^f))^T \\& + (\mathbf{I} - \mathbf{K}_k \mathbf{J}_{\mathbf{h}}(\mathbf{\mu}_{k}^f)) \mathbf{Q}_{k-1} (\mathbf{I} - \mathbf{K}_k \mathbf{J}_{\mathbf{h}}(\mathbf{\mu}_{k}^f))^T + \mathbf{K}_k \mathbf{R}_k \mathbf{K}_k^T\\ &= (\mathbf{I} - \mathbf{K}_k \mathbf{J}_{\mathbf{h}}(\mathbf{\mu}_{k}^f)) \mathbf{P}_k^f  (\mathbf{I} - \mathbf{K}_k \mathbf{J}_{\mathbf{h}}(\mathbf{\mu}_{k}^f))^T  + \mathbf{K}_k \mathbf{R}_k \mathbf{K}_k^T.\end{split}$$

In conclusion, the covariance of final estimate is the linear combination of covariances induced by the estimation based on transition from last state and the estimation from current measurements. The weights of covariances are controlled by the factor $$\mathbf{K}_k$$.

The optimal factor $$\mathbf{K}_k$$ should minimize the covariance of estimation $$\mathbf{P}_k$$. Since $$\mathbf{P}_k$$ is in square of $$\mathbf{K}_k$$, the minimum is achieved when

$$\frac{\partial tr(\mathbf{P}_k)}{\partial \mathbf{K}_k} = 0 $$

$$\begin{split}  \mathbf{K}_k &= \mathbf{P}_k^f \mathbf{J}_h^T(\mu_k^f) \left( \mathbf{J}_h(\mu_k^f)  \mathbf{P}_k^f \mathbf{J}_h^T(\mu_k^f) + \mathbf{R}_k \right)^{-1}  \\ \mathbf{P}_k &= \left( \mathbf{I} - \mathbf{K}_k (\mathbf{J}_h(\mu_k^f) \right) \mathbf{P}_k^f \end{split}$$

## A toy model

Now we show a toy example with determined model where $$\mathbf{f}(.)$$ and $$\mathbf{h}(.)$$ are both linear functions:

$$\begin{split} x_{k} &= x_{k-1} + cos(k/5) + w_{k-1} \\ z_{k} &= x_{k} + v_{k} \end{split}$$

Following the derivation above, we have the updates:

$$\begin{split} \mu_{k}^f &= \mu_{k-1}^a + cos(k/5) \\ P_{k}^f &= P_{k-1}^a + Q_{k-1} \\ K_{k} &= \frac{P_k^f}{P_k^f + R_k}   \\ \mu_k^a &= \mu_{k}^f + K_{k} (z_k - \mu_{k}^f) = (1-K_k) \mu_k^f + K_k z_k \\ P_k^a &= (1 - K_k) P_k^f = \frac{R_k P_k^f}{ P_k^f + R_k} \end{split}$$

The estimated mean value of the new state $$\mu_k^a$$ is actually the weighted average of the estimations from last state and from current measurements. The combination of two sources helps to decrease the uncertainty of estimation compared with using only one source, as $$P_k^a < P_k^f$$ and $$P_k^a < R_k$$.

## Filter Consistency

The goal of filter consistency checks is to identify when
the filter has an incorrect estimate. The validity of an
estimate can be evaluated based on whether the measurements
predicted by the estimate agree with the observed measurements.
This is typically done by monitoring the innovations
or the error residual of the filter. The consistency check assesses
whether the measured innovations follow their expected
statistical properties.

One of the most widely-used consistency measures is the Normalized
Innovations Squared (NIS) test. Specifically, a flag is thrown if
the windowed average of the NIS metric exceeds a threshold.
This test is chosen due to its robustness and ease of implementation.
We refer the readers to [here](https://pdfs.semanticscholar.org/cd84/4024e586881c666473984a2b5df89e9db457.pdf) to details.

[6] "The reasons for such a discrepancy could be either in the
prediction, the stochastic model used in the KF or in the observations."

[5] "The basic idea used to implement the filter as a detector is the fact that the innovation process is white. When the system changes due to damage the innovations are no longer white and correlation can be used to detect it."

[5] "The covariance of innovations process has significant importance for validation of the optimality of the Kalman gain, K. The consistency between theoretical covariance of innovations process and its experimental estimate demonstrates that the filter gain is optimal."

[7] "If all noise is Gaussian, the Kalman filter minimises the mean square error of the estimated parameters."

[7] "Given only the mean and standard deviation of noise, the Kalman filter is the best linear estimator. Non-linear estimators may be better."

[8] "If the measurement noise covariance parameter in a Kalman filter is too small relative to the actual noise, the filter gives too much weight to measurements relative to the process model, and estimated state trajectories are overly erratic. On the other hand, if the parameter is too large, the filter gives too little weight to measurements, and its response is sluggish."

## Reference

[1] [Extended Kalman Filter Tutorial](https://www.cse.sc.edu/~terejanu/files/tutorialEKF.pdf)

[2] [Understanding the Kalman Filter](http://www.mathstat.ualberta.ca/~wiens/stat679/meinhold&singpurwalla.pdf) An expository material laying out the derivation of kalman filter under the Bayesian formulation.

[3] [Checking consistency for Kalman filter](https://cs.adelaide.edu.au/~ianr/Teaching/Estimation/LectureNotes2.pdf)

[4] [Kalman Filters: A Tutorial](https://people.mech.kuleuven.be/~tdelaet/journalA99.pdf)

[5] [Applied Kalman filter theory (book)](http://people.duke.edu/~hpgavin/SystemID/References/Balut-KalmanFilter-PhD-NEU-2011.pdf)

[6] [Statistical Process Control of a Kalman Filter Model](https://www.mdpi.com/1424-8220/14/10/18053/pdf-vor)

[7] [Understanding and Applying Kalman Filtering](http://biorobotics.ri.cmu.edu/papers/sbp_papers/integrated3/kleeman_kalman_basics.pdf)

[8] [Online tests of Kalman filter consistency](https://tutcris.tut.fi/portal/files/3250626/KFconsistency2015.pdf)
