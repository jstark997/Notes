# Gradient Descent Convergence

It is useful to plot the value of the cost function,**J**, by the number of iterations of gradient descent to see how the value of the J changes with each iteration. This plot is called a **learning curve**.

Some key points:

- J should decrease after every iteration.
- Gradient descent has converged when the learning curve flattens out.
- Cannot tell in advance how many iterations it will take to converge.
- Once there is convergence, training of the model can stop.

## Automatic Convergene Test

Pick a number, $\epsilon$, that has some small value say 0.001.

If J decreases by $\le \epsilon$ in one iteration then declare convergence.

Picking a good value for $\epsilon$ can be difficult, and so relying on the learning curve to determine convergence is often better.
