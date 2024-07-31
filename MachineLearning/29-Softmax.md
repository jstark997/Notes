# Softmax

Is a generalization of the logistic regression algorithm to the multiclass classification case. A multiclass classification problem is an attempt to predict the value of a target that can be one of several classes.

![Softmax](/Softmax.PNG 'Softmax')

In the case where the possible outputs equals 2 (N = 2), then the softmax function reduces to logistic regression. In general the softmax function calculates a probability distribution of the targets for the given model $z$.

The softmax function can be written:
$$a_j = \frac{e^{z_j}}{ \sum_{k=1}^{N}{e^{z_k} }} \tag{1}$$
The output $\mathbf{a}$ is a vector of length N, so for softmax regression, you could also write:

$$
\begin{align}
\mathbf{a}(x) =
\begin{bmatrix}
P(y = 1 | \mathbf{x}; \mathbf{w},b) \\
\vdots \\
P(y = N | \mathbf{x}; \mathbf{w},b)
\end{bmatrix}
=
\frac{1}{ \sum*{k=1}^{N}{e^{z_k} }}
\begin{bmatrix}
e^{z_1} \\
\vdots \\
e^{z*{N}} \\
\end{bmatrix} \tag{2}
\end{align}
$$

## Loss Function

![Softmax Loss Function](/SoftmaxLoss.PNG 'Softmax loss function')

The the value of the softmax loss function depends on both the input for the training example and the value of the target for that example. For example, if the the training example has a target value of 2, then the softmax loss function would be $-log(a_2)$.

**Note** the softmax loss function, unlike the other loss functions, depends on the model values, $z$, for all the possible classes. This is because all of the possible values of $z$ are in the denominator of the softmax activation function.

## Neural Network Implementation

![Neural Network With Softmax Output Layer](/NNSoftmaxOutputLayer.PNG 'Neural network with softmax output layer')

The above is an example of the architecture for a neural network that output the probabilty that an input falls into one of 10 classes (such as one of the 10 possible digits). The hidden layers use the ReLU activation function, but the output layer has 10 units each of which uses the softmax activation function.

Each activation function in the above depends on the value of the model, $z$, for each of the other output nodes. This is different than the other activation functions which only depend on the model value for a particular node.

## Implementation in Tensorflow

![More Accurate Softmax Neural Network Implementation](/NNSofmaxMoreAccurateImplementaion.PNG 'More accurate neural network implementation with softmax in tensorflow')

The above shows how to implement softmax in tensorflow in a way that reduces the numerical rounding errors that occur if the output layer is set to use the softmax activation function directly. In the above by not calculating the acitvation function as an intermediate step, tensorflow can rearrange the terms of the loss function so as to minimize rounding errors.

![More Accurate Softmax Neural Network Implementation Prediction Correction](/NNMoreAccurateSoftmaxPrediction.PNG 'More accurate neural network implementation with softmax in tensorflow with correction for prediction')

Because the more numerically accurate implementaion sets the activation function in the output layre to the linear function, the output will be $z$ rather than $a$. In order to correct for this when the model makes predictions, neeed to apply the softmax activation function to the trained model.
