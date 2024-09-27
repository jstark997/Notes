# Gradient Descent For Linear Regression

## Linear regression model:

$f(x) = wx + b$

## Cost function (average squared error):

$J(w,b) = \frac {1}{2m} \sum_{i=1}^m{( f(x^{(i)}) - y^{(i)})^2}$

## Partial Derivative Formulas

$\frac{\partial}{\partial w}J(w, b) = \frac {1}{m} \sum_{i=1}^m{( f(x^{(i)}) - y^{(i)})x^{(i)}}$

$\frac{\partial}{\partial b}J(w, b) = \frac {1}{m} \sum_{i=1}^m{( f(x^{(i)}) - y^{(i)})}$

## Gradient descent algorithm:

repeat unitl convergence {

$w = w - \alpha \frac{\partial}{\partial w}J(w, b)$

$b = b - \alpha \frac{\partial}{\partial b}J(w, b)$

}

Where in the above the = sign is variable assignment not equality.

Replacing the partial derivatives with their formulae gives:

repeat unitl convergence {

$w = w - \alpha \frac {1}{m} \sum_{i=1}^m{( f(x^{(i)}) - y^{(i)})x^{(i)}}$

$b = b - \alpha \frac {1}{m} \sum_{i=1}^m{( f(x^{(i)}) - y^{(i)})}$

}

And replacing $f(x^{(i)})$ with its formula gives:

repeat unitl convergence {

$w = w - \alpha \frac {1}{m} \sum_{i=1}^m{( wx^{(i)} + b - y^{(i)})x^{(i)}}$

$b = b - \alpha \frac {1}{m} \sum_{i=1}^m{( wx^{(i)} + b - y^{(i)})}$

}

It is **important** to always update the parameters simulatenously, which means calculating all the partial derivates before updating any of the parameters.

**Note** that the squared error cost function is a convex function that results in a bowl shaped error surface that only has a single global minimum, and so gradient descent will not get stuck in a local minima. And, so long as the learning rate is not too large, gradient descent on a convex function will always converge on the global minimum. In this case gradient descent is _monotonic_, that is it will make steady progress toward the minimum.

## Batch Gradient Descent

In the above algorithm, when m is equal to the total number of training examples in the training set, then this is called _batch_ gradient descent. That is at each step of the gradient descent process all of the training examples are used to update the new values of the parameters.
