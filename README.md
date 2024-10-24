# Neural Networks Notes

## Contents
- [List of Symbols](https://github.com/alejandrofsevilla/neural_networks_cheat_sheet?tab=readme-ov-file#list-of-symbols)
- [Neuron Equations](https://github.com/alejandrofsevilla/neural_networks_cheat_sheet?tab=readme-ov-file#neuron-equations)
- [Training Algorithm](https://github.com/alejandrofsevilla/neural_networks_cheat_sheet?tab=readme-ov-file#training-algorithm)
- [Activation Functions](https://github.com/alejandrofsevilla/neural_networks_cheat_sheet?tab=readme-ov-file#activation-functions)
- [Cost Functions](https://github.com/alejandrofsevilla/neural_networks_cheat_sheet?tab=readme-ov-file#cost-functions)
- [Optimization Algorithms](https://github.com/alejandrofsevilla/neural_networks_cheat_sheet?tab=readme-ov-file#optimization-algorithms)
- [Regularization](https://github.com/alejandrofsevilla/neural_networks_cheat_sheet?tab=readme-ov-file#regularization)
- [References](https://github.com/alejandrofsevilla/neural_networks_cheat_sheet?tab=readme-ov-file#references)

## List of Symbols

$\large s$ *= sample*\
$\large S$ *= number of samples in training batch*\
$\large l$ *= layer*\
$\large L$ *= number of layers*\
$\large n_l$ *= neuron at layer l*\
$\large N_l$ *= number of neurons in layer l*\
$\large w_{n_{l-1}n_l}$ *= weight between neurons* $n_{l-1}$ *and* $n_l$\
$\large b_{n_l}$ *= bias of neuron* $n_l$\
$\large z_{n_l}$ *= intermediate quantity of neuron* $n_l$\
$\large y_{n_l}$ *= output of neuron* $n_l$\
$\large \hat y_{n_l}$ = *target output of neuron* $n_l$\
$\large A_{n_l}$ *= activation function at neuron* $n_l$ *{Binary Step, Linear, ReLU, Sigmoid, Tanh...}*\
$\large C$ *= cost function {MSE, SSE, WSE, NSE...}*\
$\large O$ *= optimization Algorithm {Gradient Descend, ADAM, Quasi Newton Method...}*\
$\large α$ *= learning rate*

## Neuron Equations
<p align="center">
  <img src="https://github.com/alejandrofsevilla/neural_network_notes/assets/110661590/2522d49c-d13d-4544-b7bb-59072d4dabf4" />
</p>

### Neuron Intermediate Quantity
$$ \large 
z_{n_l} = \sum_{n_{l-1}}^{N_{l-1}}(w_{n_{l-1}n_l} \cdot y_{n_{l-1}} + b_{n_l}) 
$$
### Neuron Output
$$ \large
y_{n_l} = A_{n_l}\big(z_{n_l}\big)
$$

## Training Algorithm
In order to reduce the errors of the network, weights and biases are adjusted to minimize the cost function $C$. This is done by an optimization algorithm $O$, that adjust the network parameters periodically after running a certain number of training samples.
Weights and biases are modified depending on their influence in the cost function, measured by the derivatives ${\partial C}/{\partial {w_{n_{l-1}n_l}}}$ and ${\partial C}/{\partial {b_{n_l}}}$.

$$ \large
\Delta w_{n_{l-1}n_l} = - α \cdot O\big(\frac {\partial C}{\partial {w_{n_{l-1}n_l}}}\big)
$$

$$ \large
\Delta b_{n_l} = - α \cdot O\big(\frac {\partial C}{\partial {b_{n_l}}}\big)
$$

### Chain Rule
The chain rule allows to separate the derivatives of the cost function into components:

$$ \large
\frac {\partial C}{\partial {w_{n_{l-1}n_l}}} 
= \frac{\partial C}{\partial z_{n_l}} \cdot \frac{\partial z_{n_l}}{\partial {w_{n_{l-1}n_l}}}
= \frac{\partial C}{\partial z_{n_l}} \cdot y_{n_{l-1}}
= \frac{\partial C}{\partial y_{n_l}} \cdot \frac{\partial y_{n_l}}{\partial z_{n_l}} \cdot y_{n_{l-1}}
= \dot C\big(y_{n_l}, \hat y_{n_l}\big) \cdot \dot A_{n_l}\big(z_{n_l}\big) \cdot y_{n_{l-1}}
$$

$$ \large
\frac {\partial C}{\partial {b_{n_l}}} 
= \frac{\partial C}{\partial z_{n_l}}
= \frac{\partial C}{\partial y_{n_l}} \cdot \frac{\partial y_{n_l}}{\partial z_{n_l}}
= \dot C\big(y_{n_l}, \hat y_{n_l}\big) \cdot \dot A_{n_l}\big(z_{n_l}\big)
$$

### Backpropagation
The terms $\dot C (y_{n_l} \hat y_{n_l})$ depend on the output target value for each neuron $\hat y_{n_l}$. However, a training data set only counts on the value of $\hat y_{n_l}$ for the last layer $l = L$. Instead, for all previous layers $l < L$, components $\dot C ( y_{n_l}, \hat y_{n_l})$ are computed as a weighted sum of the components previously calculated for the next layer $\dot C (y_{n_{l+1}}, \hat y_{n_{l+1}})$ :

$$ \large
\dot C \big( y_{n_l}, \hat y_{n_l} \big) = \sum_{n_{l+1}}^{N_{l+1}} w_{n_{l}n_{l+1}} \cdot \dot C \big( y_{n_{l+1}}, \hat y_{n_{l+1}} \big) 
$$

## Activation Functions
### Binary Step
<p align="center">
  <img src="https://github.com/alejandrofsevilla/neural_networks_cheat_sheet/assets/110661590/298db2aa-8a86-46c4-b59b-9312685d7ebd" alt="drawing" width="500"/>
</p>

$$ \large
\begin{split}A \big(z\big) = \begin{Bmatrix} 1 & z ≥ 0 \\
 0 & z < 0 \end{Bmatrix}\end{split}
$$

$$ \large 
\dot A \big(z\big) = 0
$$

### Linear
<p align="center">
  <img src="https://github.com/alejandrofsevilla/neural_networks_cheat_sheet/assets/110661590/63038915-6d89-47e4-ae3c-fee5258c1a5b" alt="drawing" width="500"/>
</p>

$$ \large
A \big(z\big) = z
$$

$$ \large
\dot A \big(z\big) = 1
$$

### ReLU (Rectified Linear Unit)
<p align="center">
  <img src="https://github.com/alejandrofsevilla/neural_networks_cheat_sheet/assets/110661590/49f87385-4b0f-4945-a64f-6833469d0381" alt="drawing" width="500"/>
</p>

$$ \large
\begin{split}A \big(z\big) = \begin{Bmatrix} z & z > 0 \\
 0 & z ≤ 0 \end{Bmatrix}\end{split}
$$

$$ \large
\begin{split}\dot A \big(z\big) = \begin{Bmatrix} 1 & z > 0 \\
 0 & z ≤ 0 \end{Bmatrix}\end{split}
$$

### Leaky ReLU
<p align="center">
  <img src="https://github.com/alejandrofsevilla/neural_networks_cheat_sheet/assets/110661590/3d41c6af-b516-4614-af46-1a1344738fab" alt="drawing" width="500"/>
</p>

$$ \large
\begin{split}A \big(z, \tau \big) = \begin{Bmatrix} z & z > 0 \\
\tau \cdot z & z ≤ 0 \end{Bmatrix}\end{split}
$$

$$ \large
\begin{split}\dot A \big(z, \tau \big) = \begin{Bmatrix} 1 & z > 0 \\
\tau & z ≤ 0 \end{Bmatrix}\end{split}
$$

where typically:

$$ \large \tau=0.01 $$

### Sigmoid
<p align="center">
  <img src="https://github.com/alejandrofsevilla/neural_networks_cheat_sheet/assets/110661590/644dee7c-100e-4b24-b8ba-6e2e659135a9" alt="drawing" width="500"/>
</p>

$$ \large
A \big(z\big) = \frac{1} {1 + e^{-z}}
$$

$$ \large
\dot A \big(z\big) = A(z) \cdot (1-A(z))
$$

### Tanh (Hyperbolic Tangent)
<p align="center">
  <img src="https://github.com/alejandrofsevilla/neural_networks_cheat_sheet/assets/110661590/eefc2afd-2c7c-4a2d-8ffd-c4a9efdc2c8e" alt="drawing" width="500"/>
</p>

$$ \large
A \big(z\big) = \frac{e^{z} - e^{-z}}{e^{z} + e^{-z}}
$$

$$ \large
\dot A \big(z\big) = 1 - {A(z)}^2 
$$

## Cost Functions

### Quadratic Cost
$$\large C\big(y, \hat y\big) = 1/2 \cdot {\big(y - \hat y\big)^{\small 2}}$$

$$\large\dot C\big(y, \hat y\big) = \big(y - \hat y\big)$$

### Cross Entropy Cost
$$ \large
C\big(y, \hat y\big) = -\big({\hat y} \text{ ln } y + (1 - {\hat y}) \cdot \text{ ln }(1-y)\big)
$$

$$ \large
\dot C\big(y, \hat y\big) = \frac{y - \hat y}{(1-y) \cdot y}
$$

## Regularization
Extra terms are added to the cost function in order to address overfitting.

### L2

$$ \large
C_{n_l} \big(y, \hat y\big) = C \big(y, \hat y\big) + \frac{\lambda}{2 \cdot N_{l-1}} \cdot \sum_{n_{l-1}}^{N_{l-1}} w_{n_{l-1}n_l}^2
$$

$$ \large
\dot C_{n_l} \big(y, \hat y\big) = \dot C \big(y, \hat y\big) + \lambda \cdot \sum_{n_{l-1}}^{N_{l-1}} w_{n_{l-1}n_l}
$$

### L1

$$ \large
C_{n_l} \big(y, \hat y\big) = C \big(y, \hat y\big) + \lambda \cdot \sum_{n_{l-1}}^{N_{l-1}} |w_{n_{l-1}n_l}|
$$

$$ \large
\dot C_{n_l} \big(y, \hat y\big) = \dot C \big(y, \hat y\big) ± \lambda 
$$

## Optimization Algorithms
### Gradient Descend
Network parameters are updated after every training batch $S$, averaging across all training samples.

$$ \large
O \big( \frac{\partial C}{\partial {w_{n_{l-1}n_l}}} \big) = \frac{1}{S} \cdot \sum_{s}^S{\frac{\partial C}{\partial {w_{n_{l-1}n_l}}}}
$$

### Stochastic Gradient Descend
It is a gradient descend performed after every training sample $s$.

$$ \large
O \big( \frac{\partial C}{\partial {w_{n_{l-1}n_l}}} \big) = \frac{\partial C}{\partial {w_{n_{l-1}n_l}}}
$$

### ADAM (Adaptive Moment Estimation)
Network parameters are updated after every training batch $S$, with an adapted value of the cost function derivatives.

$$ \large
O \big( \frac{\partial C}{\partial {w_{n_{l-1}n_l}}} \big) = \frac{m_t}{\sqrt{v_t}+\epsilon}
$$

where:

$$ \large
m_t = \beta_1 \cdot m_{t-1} + (1+\beta_1) \cdot \big( \frac{1}{S} \cdot \sum_{s}^S{\frac{\partial C}{\partial {w_{n_{l-1}n_l}}}} \big)
$$

$$ \large
v_t = \beta_2 \cdot v_{t-1} + (1+\beta_2) \cdot \big( \frac{1}{S} \cdot \sum_{s}^S{\frac{\partial C}{\partial {w_{n_{l-1}n_l}}}} \big)
$$

where typically:

$$ \large m_0 = 0 $$

$$ \large v_0 = 0 $$

$$ \large \epsilon = 1/10^{\small 8} $$

$$ \large \beta_1 = 0.9 $$

$$ \large \beta_2 = 0.999 $$

## References
- http://neuralnetworksanddeeplearning.com/
- https://www.analyticsvidhya.com/blog/2020/04/feature-scaling-machine-learning-normalization-standardization/
- https://comsm0045-applied-deep-learning.github.io/Slides/COMSM0045_05.pdf
- https://towardsdatascience.com/optimizers-for-training-neural-network-59450d71caf6
- https://stats.stackexchange.com/questions/154879/a-list-of-cost-functions-used-in-neural-networks-alongside-applications
- https://en.wikipedia.org/wiki/Activation_function#Table_of_activation_functions

