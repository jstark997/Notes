# Multi-Label Classification

This is where the a neural network tries to predict the classes of multiple items in the input, where each item could be one of several different classes. The output of such a neural network will be a vector of activation values (instead of a single activation value).

Below is an example of a hypothetical neural network design to classify where a picture contains a car, bus or pedestrian. This neural network has an output layer with 3 nodes, one for each label. (Since for each label the value is either 0 or 1, the sigmoid function can be used in the output layer.)

![Multi-Label Classification](/MultiLabelClassification.PNG 'Multi-Label classification')
