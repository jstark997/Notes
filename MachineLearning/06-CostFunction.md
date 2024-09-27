# Cost Function

Is a function that measures the difference, or error, between the prediction of a model and the target value.

Given a linear model $ f(x) = wx + b $ the goal is to find the parameters w and b such that $\hat{y}^{(i)}$ is close to $y^{(i)}$ for all $(x^{(i)}, y^{(i)})$ .

## Derivation of Squared Error Cost Function

Error: $( \hat{y}^{(i)} - y^{(i)})$

Squared Error (to prevent negative values from canceling out postive values): $( \hat{y}^{(i)} - y^{(i)})^2$

Sum of Squared Errors (where m is the number of training examples): $\sum_{i=1}^m{( \hat{y}^{(i)} - y^{(i)})^2}$

Average Sum of Squared Errors: $\frac 1m \sum_{i=1}^m{( \hat{y}^{(i)} - y^{(i)})^2}$

Conventional Average Sum of Squared Erros: $\frac {1}{2m} \sum_{i=1}^m{( \hat{y}^{(i)} - y^{(i)})^2}$

Squared Error Cost Function:

$J(w,b) = \frac {1}{2m} \sum_{i=1}^m{( \hat{y}^{(i)} - y^{(i)})^2}$

or alternatively since $\hat{y}^{(i)}$ is given by $f(x^{(i)}) = wx + b$

$J(w,b) = \frac {1}{2m} \sum_{i=1}^m{( f(x^{(i)}) - y^{(i)})^2}$

Most commonly used cost function for linear and most other regression problems.

## Use

The cost function is used to determine the value of the parameters that cause the model to best predict the target values.

The model will best predict the target values when the parameters **minimize** the cost function.

## Error Surface

An average squared error cost function for a single variable linear regression model will have a convex (soup bowl shaped) error surface (the plot of the values of the cost function for various values of the parameters w and b).

The minimum of this error surface can always be found by following the gradient in all dimensions.
