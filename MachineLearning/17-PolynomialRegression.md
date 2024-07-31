# Polynomial Regression

Sometimes to get a better fitting model feature engineering is used to create non-linear features from the original linear ones.

This creates polynomial models.

Example:

Original model - $y = x + 1$
Polynomial model - $y = x + x^2 + 1$

In the example the original feature was $x$ but in the polynomial model the $x^2$ term is added in an attempt to better fit the target data. This would work if the target data has more of a parabola shape.

Gradient descent in these cases will often find parameters that emphasize (are larger) those features that are more 'important' or 'correct'. In fact the features that are emphaized will be close to linear when plotted against the target values.

For polynomial regression because the range of values bewteen features will be very different feature scaling is very important in order to improve the performance of gradient descent.
