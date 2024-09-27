# Gradient Descent for Logistic Regression

The goal of the algorithm is to find the model parameters (w and b) that minimize the logistic cost function.

This is the logistic cost function:

$$J(w, b) = -\frac {1}{m} \sum_{i=1}^m{[y^{(i)}log(f(x^{(i)})) + (1 - y^{(i)})log(1 - f(x^{(i)}))]}$$

And this is the gradient descent algorithm:

repeat unitl convergence {

$w_j = w_j - \alpha \frac{\partial}{\partial w}J(w, b)$

$b = b - \alpha \frac{\partial}{\partial b}J(w, b)$

}

Where $w$ is a vector of length $n$, where $n$ is the number of features of the model.
And where $w_j$ is the parameter (weight) associated with a specific feature $j$.
The algorithm continues to update the parameters until a set of parameters is found that minimizes the logistic cost function.

Calculating the partial derivatives with repsect to w and b gives:

repeat unitl convergence {

$w_j = w_j - \alpha \frac {1}{m} \sum_{i=1}^m{( f(x^{(i)}) - y^{(i)})x_j^{(i)}}$

$b = b - \alpha \frac {1}{m} \sum_{i=1}^m{( f(x^{(i)}) - y^{(i)})}$

}

Where $m$ is the number of training examples. And $x^{(i)}$ is a vector of features of length $n$ for the $i^{th}$ training example. And $x_j^{(i)}$ is the value of the $j^{th}$ feature in $x^{(i)}$, the term that is associated with the parameter $w_j$.

Also the logistic regression model $f$ is given by:

$$f(x) = \frac{1}{1+e^{-(w \cdot x +b)}}$$

The updates to the parameters need to be done simultaneously.

The following concepts for linear regression also apply to logistic regression:
- Monitor gradient descent (evaluate the learning curve)
- Vectorized implementation
- Feature scaling

