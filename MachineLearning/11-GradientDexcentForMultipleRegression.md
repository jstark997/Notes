# Gradient Descent For Multiple Regression

## Parameters

$w = \begin{bmatrix}w_1&\cdots&w_n\\\end{bmatrix}$

$b$

## Model

$f(x) = w \cdot x + b$

## Cost Function

$J(w,b) = \frac {1}{2m} \sum_{i=1}^m{( f(x^{(i)}) - y^{(i)})^2}$

Substituting the formula for f gives:

$J(w,b) = \frac {1}{2m} \sum_{i=1}^m{((w \cdot x^{(i)} + b) - y^{(i)})^2}$

## Gradient Descent Algorithm

repeat unitl convergence {

$w_j = w_j - \alpha \frac{\partial}{\partial w_j}J(w_1,\cdots,w_n, b)$

$b = b - \alpha \frac{\partial}{\partial b}J(w_1,\cdots,w_n, b)$

}

Which is the following in vector notation:

repeat unitl convergence {

$w_j = w_j - \alpha \frac{\partial}{\partial w_j}J(w, b)$

$b = b - \alpha \frac{\partial}{\partial b}J(w, b)$

}

Substituting the formula for the partial derivatives of J gives the follownig:

repeat unitl convergence {

$w_1 = w_1 - \alpha \frac {1}{m} \sum_{i=1}^m{((w \cdot x^{(i)} + b) - y^{(i)})x_1^{(i)}}$

$\vdots$

$w_n = w_n - \alpha \frac {1}{m} \sum_{i=1}^m{((w \cdot x^{(i)} + b) - y^{(i)})x_n^{(i)}}$

$b = b - \alpha \frac {1}{m} \sum_{i=1}^m{((w \cdot x^{(i)} + b) - y^{(i)})}$

}
