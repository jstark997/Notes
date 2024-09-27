# Feature Scaling

If the features in the training set have very different ranges of values, then gradient descent can end up taking a long time to converge on the right parameters (which will also have very different ranges).

To mitigate this problem, the values of the features in the training set can be scaled to have similar ranges and therefore gradient descent should converge faster on the best parameters (which will also have similar ranges).

When features are scaled the update to parameters for each feature can make equal progress (because they within similar ranges) after each iteration of gradient descent.

## Scaling Methods

### Dividing by the Maximum

For every feature divide the value of the feature by the maximum value of that feature.

Example:

$$x_{1,scaled} = \frac{x_1}{max(x_1)}$$

### Mean Normalization

For each feature scale them so that their values are centered around zero. To do this, for each feature, find the average of the values, then for each value of the feature divide the difference between the value and the average by the difference between the maximum and minimum value for the feature.

Example:

$$x_{1,scaled} = \frac{x_1 - \mu_1}{max(x_1) - min(x_1)}$$

Where $\mu_1$ is the average of all the values of the feature $x_1$.

After scaling by mean normalization the values of a feature will usually range bewteen -1 and 1.

### Z-score Normalization

This normalizes the feature values based on a normal distribution. For each feature divide the difference between each value of the feature and the average value by the standard deviation of the feature.

Example:

$$x_{1,scaled} = \frac{x_1 - \mu_1}{\sigma_1}$$

Where $\mu_1$ is the average and $\sigma_1$ is the standard deviation of the values of the feature $x_1$.

## Rule of Thumb

In general should aim for feature values that are roughly in the range -1 to 1.

If all of the features in a training set are near that range (plus or minus a factor of 10) then rescaling is probably not necessary.

If any of the features in a training set are not close to the range -1 to 1, then rescaling is probably a good idea.

However, there is almost never any harm in performing feature scaling, so when in doubt it is best to do it.
