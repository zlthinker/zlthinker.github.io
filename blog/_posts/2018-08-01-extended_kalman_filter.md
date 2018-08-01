---
title: Extended Kalman Filter (EKF)
updated: 2018-08-01 14:00
---


* TOC
{:toc}

## Overview



## Formulation


#### Derivation
The calculus of the main component of EKF can be expressed as

$$\mathbf{x}_k = \mathbf{f}( \mathbf{x}_{k-1}) + \mathbf{w}_{k-1} , \,\,\,\,(1)$$

$$\mathbf{z}_k = \mathbf{h}( \mathbf{x}_{k}) + \mathbf{v}_{k}, \,\,\,\,(2)$$

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

In a dynamic process, the initial state $$\mathbf{x}_0$$ is known with a mean $$\mathbf{\mu}_0$$ and a covariance $$\mathbf{P}_0$$. If the process function $$\mathbf{f}(.)$$ and the observation function $$\mathbf{h}(.)$$ are linear, the Extended Kalman Filter will be equivalent to Kalman Filter and all the subsequent states are gaussian distributed. Most often, the two functions are nonlinear, so that the Extended Kalman Filter approximates them linearly by Taylor Expansion.

#### Prediction

In the new time step $$k$$, two pieces of information are available. 

(a) The first one can be obtained by Eq. (1) where current state $$\mathbf{x}_k^f$$ with covariance $$\mathbf{P}_k^f$$ is inferred from last state $$\mathbf{x}_{k-1}$$ with mean $$\mu_{k-1}^a$$ and covariance $$\mathbf{P}_{k-1}^a$$. The taylor expansion of $$\mathbf{f}(\mathbf{x})$$ at $$\mathbf{x}_{k-1}$$ gives

$$\begin{split} \mathbf{f}(\mathbf{x}_{k-1}) & \approx \mathbf{f}(\mathbf{\mu}_{k-1}^a) + \mathbf{J}_{\mathbf{f}}(\mathbf{\mu}_{k-1}^a) (\mathbf{x}_{k-1} - \mathbf{\mu}_{k-1}^a) \\ &= \mathbf{f}(\mathbf{\mu}_{k-1}^a) + \mathbf{J}_{\mathbf{f}}(\mathbf{\mu}_{k-1}^a) \mathbf{e}_{k-1}^a, \end{split}$$

where $$\mathbf{J}(.)$$ is the Jacobian function and $$\mathbf{e}_{k-1} = \mathbf{x}_{k-1} - \mathbf{\mu}_{k-1}^a$$.

$$\begin{split} \mu_k^f &= E(\mathbf{f}(\mathbf{x}_{k-1}) + \mathbf{w}_{k-1}) \\ &=  E(\mathbf{f}(\mathbf{x}_{k-1})) \\ &= E(\mathbf{f}(\mathbf{\mu}_{k-1}^a) + \mathbf{J}_{\mathbf{f}}(\mathbf{\mu}_{k-1}^a) \mathbf{e}_{k-1}) \\ &= \mathbf{f}(\mathbf{\mu}_{k-1}^a) + \mathbf{J}_{\mathbf{f}}(\mathbf{\mu}_{k-1}^a) E(\mathbf{e}_{k-1}) \\ &= \mathbf{f}(\mathbf{\mu}_{k-1}^a). \end{split}$$

The error of estimate $$\mathbf{x}_{k}$$ by $$\mu_k^f$$ is:

$$\begin{split} e_k^f & = \mathbf{x}_{k} - \mathbf{\mu}_k^f \\ &= \mathbf{f}(\mathbf{x}_{k-1}) + \mathbf{w}_{k-1} - \mathbf{f}(\mathbf{\mu}_{k-1}^a) \\ & \approx \mathbf{J}_{\mathbf{f}}(\mathbf{\mu}_{k-1}^a) \mathbf{e}_{k-1}^a + \mathbf{w}_{k-1} \end{split}$$

$$\begin{split} \mathbf{P}_k^f &= E(e_k^f {e_k^f}^T) \\ &= \mathbf{J}_{\mathbf{f}} E(e_{k-1} e_{k-1}^T) \mathbf{J}_{\mathbf{f}}^T + E(\mathbf{w}_{k-1} \mathbf{w}_{k-1}^T) \\ &= \mathbf{J}_{\mathbf{f}} \mathbf{P}_{k-1}^a \mathbf{J}_{\mathbf{f}}^T + \mathbf{Q}_{k-1}. \end{split}$$

Overall, the last state yield the gaussian estimation of current state: $$mean = \mu_k^f = \mathbf{f}(\mu_{k-1}^a)$$, $$covariance = \mathbf{P}_k^f$$.

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

$$\frac{\partial tr(\mathbf{P}_k)}{\partial \mathbf{K}_k} = 0.$$


## Reference

[1] [Extended Kalman Filter Tutorial](https://www.cse.sc.edu/~terejanu/files/tutorialEKF.pdf)
























