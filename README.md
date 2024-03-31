# Neural Networks Cheat Sheet

## Definitions:
*i = training iteration*\
*I = number of training iterations*\
*s = sample*\
*S = number of samples*\
*l = layer*\
*L = number of layers*\
$n_l$ *= neuron at layer l*\
$N_l$ *= number of neurons in layer l*\
$w_{n_{l-1}n_l}$ *= weight between neurons* $n_{l-1}$ *and* $n_l$\
$b$ *= bias of neuron* $n_l$\
$z$ *= intermediate quantity of neuron* $n_l$\
$y$ *= output of neuron* $n_l$\
$\hat y$ *= target output of neuron* $n_l$\
$A$ *= activation function at neuron* $n_l$ *{Step, Linear, ReLU, Sigmoid, Tanh...}*\
$C$ *= cost function {MSE, SSE, WSE, NSE...}*\
$O$ *= optimization function {Gradient Descend, ADAM, Quasi Newton Method...}*\
$δ$ *= error at neuron $n_l$ *\
$α$ *= learning rate*

<p align="center">
  <img src="https://github.com/alejandrofsevilla/neural_network_notes/assets/110661590/2522d49c-d13d-4544-b7bb-59072d4dabf4" />
</p>

## Neuron Equations:
### Neuron Intermediate Quantity:
$$ \begin{flalign} & z = \sum_{n_{l-1}}^{N_{l-1}}(w_{n_{l-1}n_l} \cdot y_{n_{l-1}} + b) & \end{flalign}$$
### Neuron Output:
$$ \begin{flalign} & y = A\big(z\big) & \end{flalign}$$

## Optimization Algorithm:
In order to reduce the error of the network, weights and biases are updated after every training iteration through an optimization equation $O$, which is a function of the derivatives of the cost function $C$ with respect to the network parameters.

$$ \begin{flalign} &
(w_{n_{l-1}n_l})^{i+1} = (w_{n_{l-1}n_l})^i - α \cdot O\big(\frac {\partial C}{\partial {w_{n_{l-1}n_l}}}\big)^i
& \end{flalign} $$

$$ \begin{flalign} &
(b)^{i+1} = (b)^i - α \cdot O\big(\frac {\partial C}{\partial {b}}\big)^i
& \end{flalign} $$

## Chain Rule:
The chain rule allows to separate the derivatives described above into components.

$$ \begin{flalign} &
\frac {\partial C}{\partial {w_{n_{l-1}n_l}}} 
= \frac{\partial C}{\partial z} \cdot \frac{\partial z}{\partial {w_{n_{l-1}n_l}}}
= \frac{\partial C}{\partial z} \cdot y_{n_{l-1}}
= \frac{\partial C}{\partial y} \cdot \frac{\partial y}{\partial z} \cdot y_{n_{l-1}}
= \dot C\big(y, \hat y\big) \cdot \dot A\big(z\big) \cdot y_{n_{l-1}}
& \end{flalign}$$

$$ \begin{flalign} &
\frac {\partial C}{\partial {b}} 
= \frac{\partial C}{\partial z}
= \frac{\partial C}{\partial z}
= \frac{\partial C}{\partial y} \cdot \frac{\partial y}{\partial z}
= \dot C\big(y, \hat y\big) \cdot \dot A\big(z\big)
& \end{flalign}$$

## Backpropagation
In order to compute the components $\dot C \big(y, \hat y\big)$, it would be required to have the target output for each neuron, $\hat y$. However, a training data set only counts on the value, or an estimated value of $\hat y$ for the last layer, where $l = L$. Instead, for all layers  $l < L$ , $\dot C \big(y, the components \hat y\big)$ are computed as a weighted sum of the components obtained for the following layer $\dot C \big(y_{n_{l+1}}, \hat y_{n_{l+1}}\big)$ :

$$ \begin{flalign} &
\dot C \big( y, \hat y \big) = \sum_{n_{l+1}}^{N_{l+1}} w_{n_{l}n_{l+1}} \cdot \dot C \big( y_{n_{l+1}}, \hat y_{n_{l+1}} \big) 
& \end{flalign}$$

## Optimization Function Examples:
// TODO: add examples

## Cost Function Examples:
### Mean Squared Error:

$$ \begin{flalign} &
C\big(y, \hat y\big) = \dfrac{1}{2S}\sum_{s = 1}^S \big(y - \hat y\big)^2
& \end{flalign} $$

$$ \begin{flalign} &
\dot C\big(y, \hat y\big) = \dfrac{1}{S}\sum_{s = 1}^S \big(y - \hat y\big)
& \end{flalign} $$

### Mean Binary Cross Cost
$$ \begin{flalign} &
C\big(y, \hat y\big) = -\sum_{s = 1}^S \big({\hat y} \text{ ln } y + (1 - {\hat y}) \cdot \text{ ln }(1-y)\big)
& \end{flalign} $$

$$ \begin{flalign} &
\dot C\big(y, \hat y\big) = \dfrac{1}{S}\sum_{s = 1}^S \frac{y - \hat y}{(1-y) \cdot y}
& \end{flalign} $$

// TODO: more examples

## Activation Functions:
### Binary Step:
$$ \begin{flalign} &
\begin{split}A \big(z\big) = \begin{Bmatrix} 1 & z ≥ 0 \\
 0 & z < 0 \end{Bmatrix}\end{split}
& \end{flalign} $$

$\dot A \big(z\big) = 0$

### Linear:
$A \big(z\big) = z$\
$\dot A \big(z\big) = 1$

### ReLU (Rectified Linear Unit):
$$ \begin{flalign} &
\begin{split}A \big(z\big) = \begin{Bmatrix} z & z > 0 \\
 0 & z ≤ 0 \end{Bmatrix}\end{split}
& \end{flalign} $$

$$ \begin{flalign} &
\begin{split}\dot A \big(z\big) = \begin{Bmatrix} 1 & z > 0 \\
 0 & z ≤ 0 \end{Bmatrix}\end{split}
& \end{flalign} $$

### Leaky ReLU:
$$ \begin{flalign} &
\begin{split}A \big(z\big) = \begin{Bmatrix} z & z > 0 \\
 0.01 \cdot z & z ≤ 0 \end{Bmatrix}\end{split}
& \end{flalign} $$

$$ \begin{flalign} &
\begin{split}\dot A \big(z\big) = \begin{Bmatrix} 1 & z > 0 \\
 0.01 & z ≤ 0 \end{Bmatrix}\end{split}
& \end{flalign} $$

### Sigmoid:
$A \big(z\big) = \frac{1} {1 + e^{-z}}$\
$\dot A \big(z\big) = A(z) \cdot (1-A(z))$

### Tanh (Hyperbolic Tangent):
$A \big(z\big) = \frac{e^{z} - e^{-z}}{e^{z} + e^{-z}}$\
$\dot A \big(z\big) = 1 - {A(z)}^2$

## References:
[https://en.wikipedia.org/wiki/Activation_function#Table_of_activation_functions](https://en.wikipedia.org/wiki/Activation_function#Table_of_activation_functions)\
[http://neuralnetworksanddeeplearning.com/](http://neuralnetworksanddeeplearning.com/)

