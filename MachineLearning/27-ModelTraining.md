# Model Training

Below is a comparsion of training a logistic regression model for binary classification versus a neural network.

![Comparison Training Logistic Regression with Neural Network](/ModelTrainingCompLogVsNN.PNG 'Comparing training a logistic regression model with a neural network')

The general steps are the same:

1. Create a model that outputs a prediction $y$ based on input $x$ and parameters $w$ and $b$.
2. Select a loss function and then a cost function that averages the loss over all the training examples.
3. Train the model by finding the parameters $w$ and $b$ that minimizes the cost function using an algorithm such as gradient descent.

## Loss and Cost Functions

![Loss and Cost Functions for Neural Network](/NNLossAndCostFunctions.PNG 'Loss and cost functions for a neural network')

For a binary classification problem might use a loss function such a binary cross entropy which is the same of the logistic loss function. For regression might use a different loss function such a mean squared error.

## Minimizing Cost Function by Graident Descent

![Gradient Descent for Neural Network](/NNGradientDescent.PNG 'Gradient descent for a neural network')

In a neural network the back propagation algorithm is used to calculate the partial derivatives when the gradient descent algorithm is used to minimze the cost function.
