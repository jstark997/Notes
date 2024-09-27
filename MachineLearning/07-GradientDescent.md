# Gradient Descent

Is an algorithm the can be used to find the values of the parameters of a model that minimizes its cost function.

Gradient descent can be used to minimize any function not just the cost function of a linear regression model.

## General Outline

1. Start with some initial values for the parameters (say w = 0 and b = 0).
2. Change the values of the parameters in order to reduce the value of the cost function.
3. Continue changing the parameter values unitl the cost function is at or very close to its minimum.

Depending on the cost function there may be more than 1 set of parameter values that minimizes it.

## Descent and Local Minima

Cost functions can be plotted as error surfaces given specific possible values of the parameters and the value of the cost functions at those parameters.

Gradient descent finds the steepest slope down the error surface at a given point and takes a small step down in the direction of the steepest slope.

In this way gradient descent ultimately finds a local minima.

If the error surface has more than one local minima, then local minima found by any application of gradient descent is highly dependent on the initial starting point (the initial value of the parameters).

The local minima found by a particular application of gradient descent may not be the global minimum of the cost function.

## Update Formula

To update a parameter in the gradient descent algorithm the partial derivative of the cost function with respect to the parameter multiplied by the learning rate is subtracted from the current parameter value.

For example for parameters w and b:

$w_{new} = w - \alpha \frac{\partial}{\partial w}J(w, b)$

$b_{new} = b - \alpha \frac{\partial}{\partial b}J(w, b)$

The learning rate, $\alpha$, is usually a small positive number between 0 and 1. The learning rate controls how large a step the algorithm takes down the error surface.

The parameters of a model are always updated **simulatenously** for each iteration of the algorithm. This is **important to note** because the partial derivatives with respect to the parameters need to be calculated with the current (not updated) values. So any implementation of this algorithm will likely need to use a temporary variable to store the updated value of a parameter so as to not interfere with the subsequent updates of the other parameters.

## Learning Rate

The learning rate determines how large a step down the error surface the gradient descent algorithm will take toward a local minima.

If the learning rate is **too small** then gradient descent will eventually converge on a local minama, but very slowly.

If the learning rate is **too large** then gradient descent may fail to converge (constantly overstepping the minima) and never reach a local minima.

Gradient descent can converge on a local minima even with a fixed learning rate (so long as it is not too large).

## Convergence

The algorithm converges on a local minima when the update step returns either 0 change (the partial derivative is 0) in the parameter values or only a very small change that is within some predetermined acceptable bounds.

Note as gradient descent approaches a local minima, the partial derivative in the update formula gets smaller and so the steps down the error surface also get smaller.
