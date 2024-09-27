# Neural Networks Introduction

## Basic Neural Network

![Simple Neural Network](/NeuralNetworkBasic.PNG 'Simple single layer neural network')

"To summarize, a neural network, does this, the input layer has a vector of features, four numbers in this example, it is input to the hidden layer, which outputs three numbers. I'm going to use a vector to denote this vector of activations that this hidden layer outputs. Then the output layer takes its input to three numbers and outputs one number, which would be the final activation, or the final prediction of the neural network. One note, even though I previously described this neural network as computing affordability, awareness, and perceived quality, one of the really nice properties of a neural network is when you train it from data, you don't need to go in to explicitly decide what other features, such as affordability and so on, that the neural network should compute instead or figure out all by itself what are the features it wants to use in this hidden layer. That's what makes it such a powerful learning algorithm."

## Multiple Layer Neural Network

![Multiple Layer Neural Network](/NeuralNetworkMultiLayer.PNG 'Multiple layer neural network')

## Face Recognition Example

![Face Recognition](/NeuralNetworkFaceRecognition.PNG 'Face recognition using a neural network')

"It turns out that when you train a system like this on a lot of pictures of faces and you peer at the different neurons in the hidden layers to figure out what they may be computing this is what you might find. In the first hidden layer, you might find one neuron that is looking for the low vertical line or a vertical edge like that. A second neuron looking for a oriented line or oriented edge like that. The third neuron looking for a line at that orientation, and so on. In the earliest layers of a neural network, you might find that the neurons are looking for very short lines or very
short edges in the image. If you look at the next hidden layer, you find that these neurons might learn to group together lots of little short lines and little short edge segments in order to look for parts of faces. For example, each of these little square boxes is a visualization of what that neuron is trying to detect. This first neuron looks like it's trying to detect the presence or absence of an eye in a certain position of the image. The second neuron, looks like it's trying to detect like a corner of a nose and maybe this neuron over here is trying to detect the bottom of an ear. Then as you look at the next hidden layer in this example, the neural network is aggregating different parts of faces to then try to detect presence or absence of larger, coarser face shapes. Then finally, detecting how much the face corresponds to different face shapes creates a rich set of features that then helps the output layer try to determine the identity of the person picture. A remarkable thing about the neural network is you can learn these feature detectors at the different hidden layers all by itself."
