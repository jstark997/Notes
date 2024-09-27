# Choosing The Learning Rate

With a small enough learning rate, $\alpha$, the cost function, J, should decrease on every iteration.

Otherwise the learning rate is too large.

Best to initially start with a very small learning rate and check that J is decreasing after every iteration. This helps to ensure that there are no bugs in the code.

If the learning rate is too small gradient descent will take awhile to converge.

## Algorithm

1. Start with a very small number, like 0.001, for the learning rate.
2. Run gradient descent for enough iterations to see how quickly J decreases.
3. Increase the learning rate by a factor of 3 (roughly, 0.003, 0.01, 0.03, 0.1, 0.3, 1, ...).
4. Go back to step 2.
5. Stop when J decreases very quickly without ever increasing.

In general should continue to try larger values of the learning rate until a value that is too large is discovered (that is J is increasing). That way you will know the upper bound for the learning rate.
