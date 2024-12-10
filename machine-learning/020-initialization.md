---
tags:
  - machine-learning
mathjax: true
---
# Weight Initialization
### Introduction
The first step that comes in consideration while building a neural network is the initialization of parameters - weights and biases. 
If not done correctly then layer activations might explode or vanish during the forward propagation which in turn makes loss gradients to be either too large or too small. 

Then achieving optimization will take longer or sometimes converging to a minima using gradient descent will be impossible.


### Some key points to remember
- If the weights are initialized too large or too small, the network won't learn well - because it leads to exploding or vanishing gradients problem.
- All weights should not be initialized with zeros.
  - If neurons starts with same weights, then all neurons will learn the same features and perform the same thing as one another.
  - Neural Networks try to  reach the local minima, If all the weights start at zero - it is not possible. So, it is better to give them different starting values.
 
### Weight Initialization methods
#### Normal Initialization
The authors of the famous [Alexnet Paper](https://proceedings.neurips.cc/paper/2012/hash/c399862d3b9d6b76c8436e924a68c45b-Abstract.html) initialized weights using 
zero-mean Guassian (normal) distribution with a standard deviation of 0.01. The biases were initialized as 1 for some layers and 0 for the rest.

**Uniform initialization**: bounded uniformly between ~ [ $$ \frac{-1}{\sqrt{f_{in}}}, \frac{1}{\sqrt{f_{in}}} $$ ]

But this normal random initialization of weights does not work well for training deep neural networks, because of vanishing and exploding gradient problem.

#### Xavier Initialization / Glorot initialization [[paper](http://proceedings.mlr.press/v9/glorot10a/glorot10a.pdf?hc_location=ufi])]

- Proposed by Xavier and Bengio
- considers number of input and output units while initializing weights
- weights stay within a reasonable range by making them inversely proportional to the square root of the number of units in the previous layer

**Uniform**: bounded uniformly between ~ [ $$ \pm \sqrt { \frac {6} {f_{in} + f_{out}}} $$ ]

**Normal**: multiply normal distribution by  $$ \sqrt { \frac {2} {f_{in} + f_{out}}} $$ 

  - np.random.rand(shape) * np.sqrt(  $$ \frac {2} {f_{in} + f_{out}} $$ )
  - or create normal distribution with  $$ \mu $$  = 0 and  $$ \sigma^2 $$  =  $$ \sqrt { \frac {2} {f_{in} + f_{out}}} $$ 
  
#### He initialization / Kaiming initialization [[paper](https://arxiv.org/abs/1502.01852)]
- RELU activations are mostly used - bercause they are robust to vanishing/ exploding gradients. 
- A more robust initialization technique was introduced by Kaiming et al.  for activation functions like RELU.
- both Xavier and He use similar theory →
  - find a good variance for the distribution from which the initial parameters are drawn
  - This variance is adapted to the activation function used
  - derived without explicitly considering the type of the distribution
  
    ![]({{ site.baseurl }}/images/posts/2021-02-01/int.png)
   - Red → He and Blue → Xavier
   
**Uniform**: [ $$ \pm \sqrt {\frac {6} {f_{in}} } $$ ]

**Normal**: normal distribution *  $$ \sqrt {\frac {6} {f_{in}}} $$ 

  - or  $$ \mu $$  = 0 and  $$ \sigma^2 $$  =  $$ \sqrt{\frac{2}{f_{in}}} $$ 
    
### Remember
- use Xavier for Sigmoid, tanh and Softmax 
- use He for ReLU and Leaky ReLU

### Resources
- [James Dellinger's blog post](https://towardsdatascience.com/weight-initialization-in-neural-networks-a-journey-from-the-basics-to-kaiming-954fb9b47c79)
- [How to initialize deep neural networks? Xavier and Kaiming initialization](https://pouannes.github.io/blog/initialization/)
- [Daniel Godoy's blog post](https://towardsdatascience.com/hyper-parameters-in-action-part-ii-weight-initializers-35aee1a28404)
- [Keras initializers](https://keras.io/api/layers/initializers/)
- [Pytorch forums discussion](https://discuss.pytorch.org/t/whats-the-default-initialization-methods-for-layers/3157/2)
- [Krish Naik's video](https://www.youtube.com/watch?v=tMjdQLylyGI)
