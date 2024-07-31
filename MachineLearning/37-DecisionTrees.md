# Decision Trees

Are models based on the computer science abstration called a 'tree'. The features of decision trees are represented by the decision nodes which are located at the 'root' of the tree and within the body. The predictions of a decision tree are represented by the leaf nodes at the bottom of the tree. Linking the nodes are whether the a feature is present or absent.

![Decision Tree](/DecisionTree.PNG 'Decision tree')

For any problem domain there are many possible decision trees. A decision tree learning algorithm will select one of the many possible trees that not only performs well on the training data but generalizes well on new data.

## Learning Process

1. Choose the feature to split on at each node - based on maximizing purity or minimizing impurity of the resulting subsets of the data
2. Decide when to stop:
   a. When the members of a subset are all of a single class
   b. When further splitting will result in a tree that exceeds the maximum allowable depth
   c. When further splitting results in purity improvements that are below a threshold
   d. When the number of examples available to split on is below a threshold

Items 2b, 2c and 2d are all ways to minimize the size of the decision tree and thereby mitigate overfitting.

## Measuring Purity (Entropy)

Purity is the fraction of positive examples in the subset of the training data that results from applying a decision node feature. A subset is most pure if there are only positive examples or only the absence of positive examples. The entropy function can be used as a measure of purity. This function is 0 when there are no positive examples, 1 when the subset contains 50% positive examples and 0 when there are only positive examples. So when entropy is 0 the subset is most pure.

![Entropy As Measure Of Impurity](/EntropyAsImpurityMeasure.PNG 'Entropy as a measure of impurity')

![Entropy Function](/EntropyFunction.PNG 'Entropy function')

**Note** the entropy function uses the logarithm base 2 and conventionally defines $0log_2(0)$ as equal to 0 even though the logarithm of 0 is undefined.

## Information Gain

The feature to choose to split on for a given node is the one that reduces entropy the most (gets closer to creating purer subsets). Information gain is a measure of how much a feature reduces entropy. It is computed using a weighted average of the entropy for both branches of the split, with the idea being that a larger impure sets are worse than smaller impure sets.

![Information Gain](/InformationGain.PNG 'Information gain')

The reduction in entropy is calculated by subtracting the weighted average of the entropy for both branches from the previous value of entropy (which in the above example is the entropy before the root node split).

## Decision Tree Learning Algorithm

![Decision Tree Learning Algorithm](/DecisionTreeLearningAlgorithm.PNG 'Decision tree learning algorithm')

This algorithm is a recursive algorithm.

**Note** "In theory, you could use cross-validation to pick parameters like the maximum depth, where you try out different values of the maximum depth and pick what works best on the cross-validation set. Although in practice, the open-source libraries have even somewhat better ways to choose this parameter for you."

## One-hot Encoding

Is a way of encoding a feature that has more than 2 possible values into several features that each have only 2 possible values.

![One-Hot Encoding](/OneHotEncoding.PNG 'One-hot encoding')

In general, if a feature has $k$ values then using one-hot encoding $k$ binary features (which have values either 0 or 1) can be created.

![One-Hot Encoding For Neural Networks](/OneHotEncodingForNN.PNG 'One-hot encoding for neural networks')

This technique is also used for learning algorithms that expect numbers as inputs, such as linear regression and neural networks, to transform categorical features into binary features which only take on the values 0 and 1.

## Continuous Valued Features

Decision trees can work with features that have continuous values by selecting a threshold value that produces the greatest information gain.

![Splitting On A Continuous Valued Feature](/DecisionTreeNodeContValueFeature.PNG 'Splitting on a continuous valued feature')

One way to select the threshold is to sort all of the examples according to the value of the feature and then calculate the information gain of all the values that are mid points between the sorted list of training examples. For example, if you have 10 training examples, you will test nine different possible values for this threshold and then pick the one that gives you the highest information gain.

## Regression Trees

Decision trees can be generalized to perform regression and predict values (instead of binary classification).

![Decision Tree For Regression](/DecisionTreeRegression.PNG 'Decision tree for regression')

The leaf nodes above would return a prediction based on the average of the values in the resulting subset of the training example targets.

![Decision Tree Regression Node Splitting](/DecisionTreeRegressionSplitting.PNG 'Splitting on  a node in a regression decision tree')

For a regression problem, the feature to split on is the one that reduces the _variance_ of the target value the most (instead of the entropy as in classification problems). This reduction is calculated by subtracting the weighted variance that results from a particular split from the variance before the split. The feature that has the highest reduction (highest number based on the calculation) is the one that is choosen for the split.

## Tree Ensembles

A single decision tree can be very sensitive to small changes in the data.

Using multiple decision trees (a tree ensemble) that split on different features is a solution to this problem.

![Tree Ensemble](/TreeEnsemble.PNG 'Tree ensemble')

In the above the trees are all run on the new example and the prediction selected is the one given by a majority of the trees.

## Sampling With Replacement

One technique used to build tree ensembles is sampling (the training set) with replacement. This means randomly selecting from the training set a number of examples equal to the size of the traingin set but allowing for each selected example to be available to be selected again. This will me repeats of the same training example are possible.

![Sampling With Replacement](/SamplingWithReplacement.PNG 'Sampling with replacement')

## Bagged Decision Tree

This is a type of tree ensemble that uses sampling with replacement. Sepcifically from the training set and new training set of the same size is created using sampling with replacement. This is done some number $B$ times where $B$ is typically a value between 64 and 128.

![Bagged Decision Tree](/BaggedDecisionTree.PNG 'Bagged decision tree')

## Random Forest

One problem with bagged decision trees is that often many of the trees generated have the same root node and split on the same features after the root node.

One solution to this problem is to randomize feature choice.

![Random Forest Algorithm](/RandomForestAlgorithm.PNG 'Random forest algorithm')

The sampling with replacement procedure
causes the algorithm to explore a lot of small changes to the data and to use those changes when training different decision trees. Therefore algorithms like random forest end up averaging over all of those small changes to the data that the sampling with replacement procedure causes. And so this means that any little change to the training set is less likely have a huge impact on
the overall output of the random forest algorithm, because it's already averaged over a lot of small changes to the training set.

## XGBoost

Is a modification where after the first sampling by replacement, the subsequent samplings don't select examples with equal likelihood, but instead select examples that were misclassified by previously trained trees.

The intuition behind this 'boosting' is the idea of 'deliberate practice', that is focusing the training of subsequent decision trees on a subset of the training examples that previously trained trees struggled on. Thereby accelerating performance.

![Tree Ensemble Boosting](/TreeEnsembleBoosting.PNG 'Boosting to create a tree ensemble')

The most popular open source boosting algorithm is XGBoost.

![XGBoost](/XGBoost.PNG 'XGBoost')

Technically XGBoost does not use sampling but instead assigns different weights to different training examples, and so it doesn't need to generate a lot of randomly choosen training sets making it more efficient.

Below is how the open source implemenation of XGBoost is used:

![XGBoost Implementation](/XGBoostImpl.PNG 'XGBoost implementation using open source library')

## When To Use Decision Trees

![Decision Trees Versus Neural Networks](/DecisionTreesVsNeuralNetworks.PNG 'Decision trees versus neural networks')

Neural networks work with transfer learning and this is important because for many applications there is only a small dataset and being able to use transfer learning and carry out pre-training on a much larger dataset can be critical to getting competitive performance.

When building a system of multiple machine learning models that work together, it might be easier to string together and train multiple neural networks than multiple decision trees. The reasons for this relates to the fact that even when you string together multiple neural networks you can train them all together using gradient descent, but for decision trees you can only train one decision tree at a time.
