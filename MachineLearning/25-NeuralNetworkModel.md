# Neural Network Model

The following model is of a neural network with a input layer (layer 0) a hidden layer (layer 1) and an output layer (layer 2).

![Simple Neural Network Model First Layer](/NeuralNetworkModel1.PNG 'Simple neural network model first layer')

The first layer, layer 1, takes the values of the input layer (layer 0) as a vector and for each neuron (node) in layer 1 the activation function, g, is applied to a model with parameters w and b to the input vector. The output of layer 1 is a vector of activation values.

![Simple Neural Network Model Second Layer](/NeuralNetworkModel2.PNG 'Simple neural network model second layer')

The second layer (layer 2) takes as input the vector of activation values from the first layer (layer 1) and for each neuron (in this case there is only 1) applies the activation function, g, to the model with parameters w and b to the input vector. The output of the second layer in this case is a single (scalar) activation value.

**Note** the output of each neuron is a single value (scalar), but the output of layer could be either a vector or a scalar.

A threshold could be applied to determine the class (0 or 1) that the final output falls into.

It is a convention to describe a neural network as an X layer network, where X is the number of hidden layers plus the output layer of the network. So the above example is a 2 layer network.

## General Model for a Neuron

$a_j^{[l]} = g(w_j^{[l]} \cdot a^{[l-1]} + b_j^{[l]})$

Where $l$ is the current layer, $g$ is the activation function, $w_j^{[l]}$ is the $w$ parameter for the $j^{th}$ neuron in layer $l$, $b_j^{[l]}$ is the $b$ parameter for the $j^{th}$ neuron in layer $l$, $a^{[l-1]}$ is the input vector from the previous layer and $a_j^{[l]}$ is the output value of the neuron.

**Note** The activation function, $g$, could be the logistic (sigmoid) function (which is a common choice) or some other function.

**Note** to keep the notation consistent the input layer, $x$, will also be denoted as $a^{[0]}$.
