# Bias And Variance

- High bias - model undefits the data
- High variance - model overfits the data

![Bias And Variance Examples](/BiasAndVarianceEx.PNG 'Bias and variance examples')

The training error decreases as the model better fits the training data, while the cross validation error decreases and then increases as the model better fits the training data.

![Bias And Variance Train And CV Errors](/BiasAndVarianceTrainAndCVErrors.PNG 'Bias and variance as a function of training and cross validation errors')

High bias will be indicated by a high training error. High variance will be indicated by a cross validation error that is much greater than the training error. It is possible to have a model that exhibits both high bias and high variance.

![Bias And Variance Determining](/BiasAndVarianceDetermining.PNG 'Determining bias and variance')

## Regularization

Determines, via the the regularization parameter $\lambda$, how much of a trade off there is between keeping the parameters small versus fitting the data well.

- Large value of $\lambda$ means very small parameters and possible high bias (underfit)
- Small value of $\lambda$ means very large parameters and possible high variance (overfit)

![Regularization Varying Lambda](/RegularizationVaryLamba.PNG 'Regularization with different lambdas')

Can use cross validation to pick the best $\lambda$ value.

![Regularization Choice Based On CV Error](/RegularizationChoiceCVError.PNG 'Choosing a lambda based on cross validation error')

Below is how bias and variance changes relative to $\lambda$.

![Rgularization Bias Variance And Lambda](/RegularizationBiasVarianceLambda.PNG 'Bias and variance as a funciton of lambda')

## Baseline Performance

What is the level of error you can reasonably acheive as compared to:

- Human level performance
- Competing algorithms
- Guess based on experience

Would then compare the training and cross validation errors against the baseline performance established from one of the above sources.

If the training error is significantly greater than the baseline error then the model likely has high bias.

If the training error is close to the baseline error but the cross validation error is significantly greater than the training error then the model likely has high variance.

## Learning Curves

A learning curve is a plot of how the error (either training or cross validation) changes with the size of the training set. (**Note** plotting learning curves is rarely done in practice as a way of determining whether a model has high bias or high variance.)

![Learning Curves](/LearningCurves.PNG 'Learning curves')

The cross validation error will decrease as the training set increases because the model will be better trained on more examples. But the training error will increase because it will become harder to exactly fit a model to an increasing number of training examples.

![Learning Curves High Bias](/LearningCurvesHighBias.PNG 'Learning cureves showing high bias')

If a model has high bias both the training error and cross validation error will flatten out as the training set size increases. This because the model is too simple to fit the data any better or worse. Getting more training data, by itself, won't help to improve the model.

![Learning Curves High Variance](/LearningCurvesHighVariance.PNG 'Learning curves showing high variance')

If a model has high variance there will be a large gap between the training error and cross validation error (with the baseline error likely falling in between the two). However, getting more training data, by itself, could improve the model as the training error increases thereby decreasing overfit. This will also likely reduce the cross validation error.

## Debugging For Bias And Variance

![Bias And Variance Debugging](/BiasAndVarianceDebugging.PNG 'Debugging for bias and variance')

If the model has high variance then either need to get more training examples or need to simplify the model.

If the model has high bias then need to make the model more complex.

## Bias And Variance In Neural Networks

Traditionally there is a tradeoff between the complexity of a model (number of features) and bias and variance.

Neural networks don't suffer as much from this tradeoff. In fact large neural networks are low bias machines.

Below is illustrated an algorithm for developing a neural network that has low bias and low variance:

![Neural Network Bias And Variance](/NeuralNetworkBiasAndVariance.PNG 'Neural network development algorithm for bias and variance')

A large neural network will usually do as well or better than a smaller one so long as regularization is choosen appropriately.
