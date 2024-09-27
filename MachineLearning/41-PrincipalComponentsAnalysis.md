# Principal Components Analysis (PCA)

A method is reduce the number of features from possibly 1000s to just 2 or 3. Commonly used in data science for visualization.

![PCA Example Car Size](/PCAExampleCarSize.PNG 'PCA example car size')

In the above example, both the height and length of cars vary by a significant amount. In order to reduce the number of features from 2 to just 1, the combination of the height and length of a car can be taken as a new feature that represents an important identifying attribute of a car. This feature would lie along a different axis than the height or length feature.

Below is an example of 3 dimensional data that PCA has reduced to 2 dimensions.

![PCA Example 3D To 2D View 1](/PCAExample3DTo2DView1.PNG 'PCA example 3D to 2D view 1')

Looked at from another angle, we can see that the 3 dimensional data is actually clustered in a plane that is relatively easily represented in 2 dimensions.

![PCA Example 3D To 2D View 2](/PCAExample3DTo2DView2.PNG 'PCA example 3D to 2D view 2')

In the example below, 50 features about countries has been reduced to just 2.

![PCA Example Countries Feature Reduction](/PCAExampleCountriesFeaturesReduction.PNG 'PCA example countries features reduction')

Using the reduced number of features for the countries above it becomes much easier to visualization the data in a meaningful way.

![PCA Example Countries Visualization](/PCAExampleCountriesVisualization.PNG 'PCA example countries visualization')

## PCA Algorithm

Before applying the PCA algorithm it is important to normalize the features to have a zero mean and to have the same scale.

![PCA Preprocess Features](/PCAPreprocess.PNG 'PCA preprocess features')

The PCA algorithm finds the axis where when the data is projected onto this axis the variance between the data is maximized and hence the information about the data is maximally preserved.

![PCA Choose Axis Example 1](/PCAChooseAxis1.PNG 'PCA choose axis example 1')

The above choice of an axis is not too bad as the a fair amount of the variance between the data is preserved.

![PCA Choose Axis Example 2](/PCAChooseAxis2.PNG 'PCA choose axis example 2')

The choice of the axis above is not great because the variance between the data is significantly reduced.

![PCA Choose Axis Example 3](/PCAChooseAxis3.PNG 'PCA choose axis example 3')

The above is the best choice for an axis given this data set as it preserves the maximum amount of variance between the data. This varaince maximizing axis is called a principal component.

Projecting data onto an axis is calculated by the dot product of the vector of the data to be projected with the unit vector of the axis to be projected onto, as illustrated below.

![PCA Projecting Onto Axis](/PCAProjectingOntoAxis.PNG 'PCA projecting onto axis')

There can be more than a single principal component (representing more than one feature to visualize). Each principal component will be perpendicular to the rest.

![PCA More Principal Components](/PCAMorePrincipalComponents.PNG 'PCA more principal components')

Given the value of the data on the principal component, an approximation of the original data points can be derived by multiplyig the principal component value by the principal component unit vector. This is called reconstruction. Reconstruction will give an appproximation of the original values, but is unlikely to return the exact original values.

![PCA Reconstruction](/PCAReconstruction.PNG 'PCA reconstruction')

## PCA Implementation Using Scikit Learn

![PCA Scikit Learn Steps](/PCAScikitLearnSteps.PNG 'PCA scikit-learn steps')

The example below takes 2 dimensional data and reducees it to 1 dimension. The principal component in this example captures or explains 99.2% of the variance in the data. 

![PCA Scikit Learn Example](/PCAScikitLearnExample.PNG 'PCA scikit-learn example')

Today PCA is most often used for data visualization. However in the past is was used for data compression and to reduce the number of features to speed up supervised learning.

![PCA Applications](/PCAApplications.PNG 'PCA applications')





