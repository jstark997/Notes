# Regularization

Used to prevent overfitting without resorting to feature selection. Systematically reduces the impact of all the features in the model by making the parameters smaller than they otherwise would be without regularization.

Below is the linear regression cost function with regularization:

$J(w,b) = \frac {1}{2m} \sum_{i=1}^m{( f(x^{(i)}) - y^{(i)})^2 + \frac{\lambda}{2m}\sum_{j=1}^n{w_j^2}}$

Where $\lambda$ is the **regularization parameter** and is set to some value greater than 0.

**Note** $\lambda$ is divided by $2m$ so that it scales in the same way as the first means squared error term. This tends to make values of $\lambda$ scale with changes to the size of the training set.

Also it is common practice not to regularize the parameter $b$ because doing so tends to make little difference.

Minimizing the mean squared error term fits the model to the data as well as possible, while minimizing the regularization term helps to prevent overfitting. These are competing goals and represent a basic trade off in machine learning. The value of $\lambda$ represents the relative importance of fit versus the risk of overfitting.

If the value of $\lambda$ is 0, then there will be no regularization and the model is at risk of overfitting. If, howver, $\lambda$ is some very large value then the minimizing (gradient descent) process will cause the value of the $w$ parameters to be very close to 0 resulting in a a straight line that underfits the data.

## Gradient Descent for Linear Regression with Regularization

The gradient descent algorithm remains the same:

repeat unitl convergence {

$w_j = w_j - \alpha \frac{\partial}{\partial w_j}J(w, b)$

$b = b - \alpha \frac{\partial}{\partial b}J(w, b)$

}

What changes is the partical derivative with respect to $w$ which is given by:

$\frac{\partial}{\partial w_j}J(w, b) =  \frac{1}{m} \sum_{i=1}^m{( f(x^{(i)}) - y^{(i)})x_j^{(i)}} + \frac{\lambda}{m}w_j$

The partial derivate with respect to $b$ remains the same:

$\frac{\partial}{\partial b}J(w, b) = \frac{1}{m} \sum_{i=1}^m{( f(x^{(i)}) - y^{(i)})}$

## Gradient Descent for Logistic Regression with Regularization

The algorithm is the same as for linear regression but with $f(x)$ replaced by the logistic function.
