# Adam Algorithm

A modification of the gradient descent algorithm that increases or decreases the learning rate depending on how gradient descent is proceeding.

Tends to work faster than regular gradient descent and has become the def facto standard.

![Adam Gradient Descent Modification](/AdamAsGradientDescentMod.PNG 'Adam algorithm is a modification of gradient descent')

## Details and Intuition

The Adam algorithm uses a different learning rate for each parameter, which allows it to fine tune the rate based on the progress of each parameter.

![Adam Multiple Learning Rates](/AdamMultiLearningRate.PNG 'Adam algorithm uses multiple learning rates')

If the gradient of a parameter keeps moving in the same direction, then the Adam algorithm will increase the learning rate for that parameter.

But if the gradient of a parameter keeps oscillating, then the Adam algorithm will decrease the learning rate for that parameter.

![Adam Learning Rate Adjustment](/AdamLearningRateAdjust.PNG 'Adam learning rate adjustment per parameter')

## Implementation

![Adam Implementation Tensorflow](/AdamImplementationTensorflow.PNG 'Adam implementation in tensorflow')

The Adam algorithm is set in tensorflow with the 'optimizer' parameter in the compile method. An initial learning rate needs to be specified, and the Adam algorithm, because it automatically adapts the learning rate, it is robust as to which initial learning rate is selected, however, it is worth experimenting with different values to see which works best.
