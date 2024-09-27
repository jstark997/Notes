# Classification

**Note** the terms _class_ and _category_ are used interchangeably.

## Binary Classification

This is where there are only 2 classes that a target value can be:

- no or yes
- false or true
- 0 or 1

Examples:

- Is this email spam?
- Is the transaction fraudulent?
- Is the tumor malignant?

Often the values 0 or 1 are choosen because they are most useful for running classification algorithms.

The values no, false and 0 are often referred to as the 'negative class', and the values yes, true and 1 are referred to as the 'positive class'. The negative class denotes the absence of something, whereas the positive class denotes the presence of something.

Linear regression does not work well for this type of classification. If data is tightly clustered it might seem that linear regression with a reasonable threshold value would work, but is a data point is added that is far from the others this will cause the best fitting line to shift and along with it the decision boundary (based on the threshold value). This shift will likely lead to errors in classification. The algorithm that avoids this problem is _logistic regression_.
