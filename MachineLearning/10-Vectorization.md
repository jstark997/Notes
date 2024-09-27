# Vectorization

Expressing a regression problem in terms of vectors allows the use of libraries that are very efficient and make it easier to code regression solutions.

**Note** The index for elements in a vector expressed in mathematics starts at 1, but for vectors expressed in Python (and most other programming languages) the index starts at 0.

## Without Vectorization

Where n = 3.

Model:

$f(x) = w_1x_1 + w_2x_2 + w_3x_3 + b$

or alternatively

$f(x) = (\sum_{j=1}^3{w_jx_j}) + b$

Code:

```python
n = 3
f = 0
for j in range(0,n):
  f = f + w[j] * x[j]
f = f + b
```

For small values of n this might be okay, but for large values of n this is not very efficient.

## With Vectorization

Parameters and features where n = 3:

$w = \begin{bmatrix}w_1&w_2&w_3\\\end{bmatrix}$

b is a number

$x = \begin{bmatrix}x_1&x_2&x_3\\\end{bmatrix}$

Model:

$f(x) = w \cdot x + b$

Code:

```python
w = np.array([1.0,2.5,-3.3])
b = 4
x = np.array([10,20,30])

f = np.dot(w,x) + b
```

The above uses the Numpy library to calculate the cost function in a single line of code (using the dot product of the vectors w and x) and is very efficient (even for large values of n).

## Advantages of Vectorization

1. Able to more succinctly express regression problems in code.
2. Libraries such as Numpy that depend on vectorization are able to execute operations on even large vectors very efficiently (by using parallelization).
