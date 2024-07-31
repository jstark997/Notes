# Activation Functions

There are several choices for the activation function:

1. Logistic (Sigmoid)
2. Rectified Linear (ReLU)
3. Linear

![Activation Functions](/NNActivationFunctions.PNG 'Activation functions')

# Why Use An Activation Function

Using the appropriate activation funciton to modify the output of the model is what allows a neural network to learn features (in the hidden layers).

Not using an activation function or using only the linear activation function will reduce the neural network to linear regression.

If all the hidden layers use the linear activation function but th output layer uses the logistic (sigmoid) activation function, then the neural network reduces to logistic regression.

![Why Use Activation Functions](/NNWhyUseActivationFunctions.PNG 'Why use activation functions')

## Choosing An Activation Function

Choosing an activation function for the output layer depends on the range of values the target might have:

![Activation Function Choice Output Layer](/NNActivationFuncChoiceOutputLayer.PNG 'Activation function choice output layer')

For the hidden layers the most common and usually best choice is the ReLU activation function:

![Activation Function Choice Hidden Layer](/NNActivationFuncChoiceHiddenLayer.PNG 'Activation function choice hidden layer')

As compared to the sigmoid function (which was commonly used in the past), the ReLU function can learn faster because it tends to generate fewer areas in the cost function where the gradient is very low (or near flat).

Below is a quick summary of which acitvation functions to choose for differen scenarios:

![Activation Function Choice Summary](/NNActivationFuncChoiceSummary.PNG 'Activation function choice summary')

**Note** despite what the summary indicates, the linear activation funcion is sometimes used for the output layer where the target ranges from 0 to positive values.
