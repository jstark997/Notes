# Overfitting and Underfitting

The goal of machine learning is to produce models that _generalize_ well. That is models that are able to make good predictions on data not in the training set.

## Underfitting (High Bias)

When a model does not fit the training data very well. This is also known as _high bias_. That is the model is biased toward fitting the data to an incorrect pattern. In this case the model does not make good predictions, even on data in the training set.

Often the result of having **too few features**.

## Overfitting (High Variance)

When a model fits the training data perfectly or almost perfectly (this cost function is zero or near zero for all of the training data). This is also known as _high variance_. That is the model parameters would vary quite a bit when trained on different data sets. Whereas the parameters of a model that is generalizable should not vary much when trained on different data sets (from the same domain). In this case even though the model makes perfect predictions on the training data it makes poor predictions on data outside the training set.

Often the result of having **too many features**.

## Fixing Overfiiting

### 1. Collect More Training Data

With more training examples a model that overfit the initial training set, when trained on the new augmented set, might turn out to generalize well.

### 2. Feature Selection

A combination of insufficient training data and all lot of features could result in an overfitting model. Reducing the number of features (using intuition to pick a subset that is likely to be most predictive) could help the newly trained model generalize better. However, there is the risk that the features exclude include useful (for prediction) data.

### 3. Regularization

Sets the parameter values for some features to a small number to reduce their impact in the model. This has the benefit of improving generalization without completely removing any features.
