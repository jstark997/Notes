# Logistic Regression

In binary classification, used to fit an S shaped curve to the data where the middle of the curve is a threshold or the boundary value between the positive and negative classes.

## Sigmoid Function

Also referred to as the logistic function.

This function for a given input value ouputs values between 0 and 1.

$$g(z) = \frac{1}{1+e^{-z}}$$

Where $0 < g(z) < 1$

A graph of this function with z on the horizontal axis and g(z) on the vertical axis results in an S shaped curve with the middle of the curve passing through 0.5 when z = 0.

When x is positive the value of the function approaches 1 and when x is negative the value of the function aoproaches 0.

## Model

The linear regression model below is used as the input to the logistic function to create the logistic regression model:

$$z = f(x) = w \cdot x + b$$

$$g(z) = \frac{1}{1+e^{-z}}$$

$$g(w \cdot x + b) = \frac{1}{1+e^{-(w \cdot x +b)}}$$

Where w and b are the parameters and w and x are vectors.

In essence the sigmoid function is used to transform the output of the linear regression model from a wide range of values to the narrow range of values between 0 and 1.

## Interpreting the Ouput of the Model

The output of the logistic regression model can be interpreted as the 'probability' that the input is in the positive class.

## Decision Boundary

The logistic regression model outputs values between 0 and 1, but for classification we need to predict target values of either 0 or 1.

In order to achieve this we can set a threshold value above which the predicted target value will be 1 and below which the predicted target value will be 0.

One obvious threshold value, given the shape of the sigmoid function is 0.5.

The predicted target value is denoted by $\hat{y}$, and so given the logistic regression model:

$$f(x) = g(z) = g(w \cdot x + b) = \frac{1}{1+e^{-(w \cdot x +b)}}$$

and a threshold of $0.5$ we have:

$$\hat{y} = 1 \text{ when } f(x) \ge 0.5 \text{ and } \hat{y} = 0 \text{ when } f(x) \lt 0.5$$

When is $f(x) \ge 0.5$?

When $g(z) \ge 0.5$

Which is when $z \ge 0$

Which is when $w \cdot x + b \ge 0$ and therefore, with a threshold of $0.5$, $\hat{y} = 1$.

The decision boundary therefore is given by the line:

$$z = w \cdot x + b = 0$$

**Important** - The threshold value depends on the application to which the logistic regression model is being applied. It does not have to be 0.5.

## Non-Linear Decision Boundary

The input to the logistic regression model, z, is a linear regression model. However, just as linear regreesion models can have polynomial terms, the input to a logistic regression model can also be a polynomial. This will result in non-linear decision boundaries.
