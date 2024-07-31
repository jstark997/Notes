## Logistic Loss Function With Less Rounding Errors

Because computers have limited memory, different mathematically equivalent calculations for the same value can result in more or less rounding errors.

Below is an example of a way to reduce the rounding errors for a neural network that uses the logistic loss function for the output layer:

![Logistic Loss Function With Less Rounding Errors](/NNLogisticLessRoundingErrors.PNG 'Neural network implemenation in ternsorflow with logistic loss function with less rounding errors')

In the above by substituting the intermediary calculation of the logistic activation function with the direct calculation in the loss function, tensorflow can rearrange the terms of in the loss function to reduce rounding errors.

In tensorflow the sigmoid activation function is replaced with the linear activation function in the output layer and the loss parameter is BinaryCrossEntropy with the 'from_logits' parameter set to true.

![More Accurate Logistic Neural Network Prediction](/NNMoreAccurateLogisticPrediction.PNG 'Neural network implemenation for logistic classification including correction for prediction')

Because the output layer uses the linear activation function, in order to get appropriate predications (because the output layer returns $z$ and not $a$), need to apply the logistic function to the output of the trained model.
