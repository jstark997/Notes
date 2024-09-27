# Multiple Features

Linear regression can be performed on data that inclues more than 1 feature.

Data that has more than 1 feature can be represented as a matrix.

Each row of the matrix is a single training example.

Notation:

- $x_j$ = $j^{th}$ feature
- n = number of features
- $x^{(i)}$ = vector of features for $i^{th}$ training example
- $x_j^{(i)}$ = value of feature j in $i^{th}$ training example
- $w^{(i)}$ = vector of parameters for $i^{th}$ training example
- b = the base parameter

## Multiple Linear Regression Formula

With n features the formula for multiple linear regression is:

$f(x) = w_1x_1 + w_2x_2 + \cdots + w_nx_n + b$

Or using vectors for w and x:

$f(x) = w \cdot x + b$

Where in the above the dot product of w and x is taken and then b is added to it.
