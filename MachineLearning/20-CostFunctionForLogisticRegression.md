# Cost Function for Logistic Regression

Is a function that measures the difference, or error, between the prediction of a model and the target value. Used to evaluate the model with its current paraemeters.

## Mean Squared Error Not A Good Cost Function For Logistic Regression

The linear regression model is:

$$f(x) = wx + b$$

The cost function for linear regression is the mean squared error:

$$J(w,b) = \frac {1}{2m} \sum_{i=1}^m{( f(x^{(i)}) - y^{(i)})^2}$$

The error surface that results from the above cost function is convex and therefore is ideal for gradient descent to find the parameters that minimize the error of the model.

However, the logistic regression model is:

$$f(x) = \frac{1}{1+e^{-(w \cdot x +b)}}$$

Plugging this into the mean squared error cost function results in a non-convex error surface with many local minima that is not suited for the gradient descent algorithm.

## Linear Loss Function

Take the cost function for linear regression:

$$J(w,b) = \frac {1}{2m} \sum_{i=1}^m{( f(x^{(i)}) - y^{(i)})^2}$$

And modify it slightly to make the math a bit easier by moving the $\frac{1}{2}$ term inside the summation (which does not change the result):

$$J(w,b) = \frac {1}{m} \sum_{i=1}^m{\frac{1}{2}( f(x^{(i)}) - y^{(i)})^2}$$

The term inside the summation of this cost function is referred to a the _loss_:

$$L(f(x^{(i)}),y^{(i)}) = \frac{1}{2}( f(x^{(i)}) - y^{(i)})^2$$

More specifically _loss_ is a measure of the difference of a single example to its target value, while _cost_ is a measure of the losses over the entire training set.

## Logistic Loss Function

$$
L(f(x^{(i)}),y^{(i)}) =
\left\{
\begin{array}{l}
-log(f(x^{(i)}))\text{       if } y^{(i)} = 1 \\
-log(1 - f(x^{(i)}))\text{   if } y^{(i)} = 0
\end{array}
\right.
$$

Or alternatively:

$$L(f(x^{(i)}),y^{(i)}) = (-y^{(i)}log(f(x^{(i)})) - (1 - y^{(i)})log(1 - f(x^{(i)})))$$

In the alternative formulation when $y^{(i)}$ is 0 the left term is 0 and when $y^{(i)}$ is 1 the right term is 0.

Given the above the loss is lowest when the model $f(x^{(i)})$ predicts a value close to the true label $y^{(i)}$.

If the true label is 1 then the loss function $-log(f(x^{(i)}))$ applies. In this case if the model predicts a value close to 1 the loss will be close to 0, but if the model predicts a value closer to 0 the loss will approach $\infty$.

If the true label is 0 then the loss function $-log(1-f(x^{(i)}))$ applies. In this case if the model predicts a value close to 0 the loss will be close to 0, but if the model predicts a value closer to 1 the loss will approach $\infty$.

The big advantage of this loss function for logistic regression is that it makes the overall cost function for logistic regression convex which means that the gradient descent algorithm can be used to find a global minimum.

## Simplified Cost Function

Logistic loss function:

$$L(f(x^{(i)}),y^{(i)}) = (-y^{(i)}log(f(x^{(i)})) - (1 - y^{(i)})log(1 - f(x^{(i)})))$$

Logistic cost function:

$$J(w, b) = \frac {1}{m} \sum_{i=1}^m{L(f(x^{(i)}),y^{(i)})}$$

Substituting the logistic loss function into the above (and moving the negative sign outside of the summation) gives:

$$J(w, b) = -\frac {1}{m} \sum_{i=1}^m{[y^{(i)}log(f(x^{(i)})) + (1 - y^{(i)})log(1 - f(x^{(i)}))]}$$

Note, the above cost function is derived from the statistical principle of maximum likelihood estimation.
