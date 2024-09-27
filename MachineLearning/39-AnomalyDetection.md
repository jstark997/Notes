# Anomaly Dectection

Anomaly detection algorithms learn models on unlabelled 'normal' data. The learned models then can be used on new data to detect unusual or anomalous data.

The most common way to detect anomalies is using a technique called _density estimation_.
The model learned by the anomaly detection algorithm creates a function $p(x)$ that returns the probability that $x$ would be in a normal dataset. If $p(x)$ is smaller than some threhold value $\epsilon$ then $x$ is considered to be anomalous.

![Density Estimation](/DensityEstimation.PNG 'Density estimation')

## Examples

![Anomaly Detection Examples](/AnomalyDetectionExamples.PNG 'Anomaly detection examples')

## Gaussian (Normal) Distribution

A Gaussian or normal distribution is a probability distribution of the values of a random variable that is symmetric around the mean, $\mu$. A specific distribution is defined by the mean, $\mu$ and the variance, $\sigma^2$. The standard deviation is the square root of the variance or $\sigma$. Because the distribution is a probability distribution the area under a graph of the distribution sums to 1.

![Gaussian Distribution](/GaussianDistribution.PNG 'Gaussian or normal distribution')

Changing the value of the mean changes the point on along the x-axis that a graph of the distribution is symmetric around. Whereas changing the value of the standard deviation changes the width of the graph (which has to be the case since the area under the graph must sum to 1).

![Gaussian Distribution Examples](/GaussianDistributionExamples.PNG 'Gaussian distribution examples')

## Parameter Estimation

Estimating the parameters $\mu$ and $\sigma^2$ from a training dataset for the purpose of anomaly detection can be done by calculating the average of the training examples to get $\mu$ and the mean squared difference from the mean of the training examples to get $\sigma^2$. These calculations are called the maximum likelihood estimates of $\mu$ and $\sigma$.

_Note_ technically according to statistics, because the training examples are usually a sample from a population and not the entire population of examples, instead of $\frac{1}{m}$ the value $\frac{1}{m-1}$ should be used. However, in practice it does not matter.

![Anomaly Detection Parameter Estimation](/AnomalyDetctionParameterEstimation.PNG 'Parameter estimation for anomaly detection')

Using the maximum likelihood calculations for $\mu$ and $\sigma$ will result in a Gaussian distribution that can be used to determine the probability of a new data point having a particular value. If the probability for the new data point is low, then it could be considered an anomaly.

## Anomaly Detection Algorithm

For a dataset that has multiple features, the anomaly detection algorithm performs density estimation by estimating the parameters $\mu$ and $\sigma^2$ for each feature and then multiplying the probabilty density functions that result for each feature.

![Density Estimation Multiple Features](/DensityEstimationMultipleFeatures.PNG 'Density estimation multiple features')

Multiplying the probability density function of each feature implicitly assumes that each feature is independent of the others. In practice, however, multiplying the probability density functions to get a density estimate for the data works well even if the features are not independent of each other.

Below is the general anomaly detection algorithm:

![Anomaly Detection Algorithm](/AnomalyDetectionAlgorithm.PNG 'Anomaly detection algorithm')

This algorithm will create a probability density function over all the features in the training data, and will return a low probability value for any data point that has a feature value that is either very large or very small compared to the data points in the training set.

Below is an example of how to use the density estimate to detect anomalous values for a dataset that has 2 features:

![Anomaly Detection Density Estimate Example](/AnomalyDetectionDensityEstExample.PNG 'Anomaly detection density estimate example')

## Developing An Anomaly Detection System

Need a way to evaluate the performance of an anomaly detection algorithm when some parameter, such as $\epsilon$, changes, by computing a number that indicates whether the algorithm has gotten better or worse based on the change. This is called _real number evaluation_.

In order to do this evaluation there will need to be a cross-validation and test sets that have some anomalies and where each example in the set is labeled as either normal, 0, or anomalous, 1.

![Anomaly Detetion Real Number Evaluation](/AnomalyDetectionRealNumberEvaluation.PNG 'Real number evalution in anomaly detection')

Below is an example of real number evaluation:

![Anomaly Detection Real Number Evaluation Example](/AnomalyDetectionRealNumberEvalExample.PNG 'Example of real number evaluatoin for anomaly detection')

Below is a summary of the anomaly detection evaluation algorithm:

![Anomaly Detection Evaluation Algorithm](/AnomalyDetectionEvaluationAlgorithm.PNG 'Anomaly detection evaluation algorithm')

**Note** notice the similarity of the above with evaluating skewed datasets.

## Anomaly Detection Versus Supervised Learning

Anomaly detection tries to model what is 'normal' in a dataset, from negative examples, and then flag data that is outside of normal (positive examples). Whereas surpervised learning tries to learn a model that can accurately predict future positive examples.

Below is a summary of when to choose anomaly detection or supervised learning:

![Anomaly Detection Versus Supervised Learning](/AnomalyDetectVsSupervisedLearn.PNG 'Anomaly detection versus supervised learning')

## Choosing Features

Important to choose the right features for anomaly detection.

Features should have a Gaussian distribution.

For features that don't have a Gaussian distribution, should see if that feature can be transformed into a Gaussian distribution.

![Anomaly Detection Non-Gaussian Features](/AnomalyDetectionNonGaussianFeatures.PNG 'Transforming non-Gaussian features for anomaly detection')

If after evaluating the model it is not performing well, then can try an error analysis. That is take a look at an anomalous data point that was not flagged and see if there is some other feature that can be added that would distinguish it from the other normal data points. Such a new feature would take on either an unusually large or small value when there is an anomaly (making its probability of occurence low).

![Anomaly Detection Error Analysis](/AnomalyDetectionErrorAnalysis.PNG 'Error analysis of anomaly detection model')
