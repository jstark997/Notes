## Tensorflow

Is a framework for building neural networks in Python.

## Syntax Example - Model for Digit Classification

![Tensorflow Model Digit Classification](/TensorflowModelDigitClass.PNG 'Tensorflow model for digit classification')

The function 'Dense' specifies the type of the layer.

## Numpy Array and Tensor

The Numpy and Tensorflow libraries represent matrices differently. In Numpy matrices are 2D arrays where each element is a row in the matrix. In Tensorflow there is a object type called a 'Tesnor' that represents matrices. Tensorflow requires input vectors to be matrices (and hence only 2D Numpy arrays can be used).

![Tensorflow Activation Vector Layer 1](/TensorflowActivationVec1.PNG 'Tensorflow activation vector layer 1')

In the above a 2D Numpy array is used as the input vector. When the layer 1 activation vector is displayed it is a Tensor object. This Tensor object can be converted into a 2D Numpy array by calling the 'numpy' method.

![Tensorflow Activation Vector Layer 2](/TensorflowActivationVec2.PNG 'Tensorflow activation vector layer 2')

In the above layer 2 has a single neuron so the output is essentially a scalar. However, Tensorflow represents as a 1 x 1 matrix.

## Building a Neural Network

![Tensorflow Building A Neural Network](/TensorflowBuildNN.PNG 'Tensorflow building a neural network')

In the above the function 'Sequential' is called to connect the layer 1 and layer 2 of the neural network defined by the 'Dense' function, so that the activation vector of layer 1 is feed into the layer 2 during the inference process.

The input vector is a 2D Numpy array and the target vector is a 1D Numpy array.

The function 'complie' is called to compile the model with the parameters specified in the arguments.

The function 'fit' is called with the input vector and target vector as arguments to train the neural network.

The function 'predict' is called with an input vector of new data to perform forward propagation inference and make a prediction.

## Train a Neural Network

![Tensorflow Train A Neural Network](/TensorflowTrainNN.PNG 'Tensorflow training a neural network')

The above is the same as building a neural network. The key parameters are the loss function which is specified in the compile method called on the model, and the number of training epochs specified in the call to the fit method.
