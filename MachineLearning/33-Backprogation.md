# Back Propagation

The algorithm that calculates the partial derivatives used in gradient descent (and Adam) to minimizes the cost function and thereby train a neural network.

## Computational Graph

"The computation graph is a key idea in deep learning, and it is also how programming frameworks like TensorFlow, automatic compute derivatives of your neural networks."

![Computational Graph Forward Propagation](/ComputationalGraphForwardProp.PNG 'Computational graph showing forward propagation')

The above shows a graph of the nodes in the computation of the forward propagation inference for a very small neural network.

![Computational Graph Back Propagation](/ComputationalGraphBackProp.PNG 'Computational graph showing back propagation')

The above shows the computational graph for the same small neural network, but also shows how by going right to left on the graph (back propagation) the derivatives of the cost function can be calculated (using the chain rule of calculus).

Steps:

1. Calculate the local derivative(s) of the node.
2. Using the chain rule, combine with the derivative of the cost with respect to the node to the right.

![Computational Graph Efficiency](/ComputationalGraphEfficiency.PNG 'Computational graph showing the efficiency of back propagation')

The reason back propagation is used to calculate the derivatives of the cost function is that it is efficient. The derivative at each node in the graph only needs to be calculated once.

It is possible to use forward propagation to determine how much the cost function changes if the parameters change by a very small amount, $\epsilon$, but then the number of calculations becomes $N \times P$, where $N$ is the number of nodes and $P$ is the number of parameters, which is significantly less efficient than back propagation.
