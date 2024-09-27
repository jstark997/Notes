# Recommender Systems

A system that recommends items to users based on what the user liked, viewed or purchased before.

## Per Item Recommendation Prediction With Features

![Per Item User Rating Prediction Example](/PerItemUserRatingPredictionEx1.PNG 'Example of how to predict user rating per item using features')

![Per Item User Rating Cost Function One User](/PerItemUserRatingPredictionCostFunctionOneUser.PNG 'Cost function for per item user rating perdiction for one user')

In the above the number of items rated by the user $m^{(j)}$ is a constant and so it is removed from the cost function as it does not have an affect.

![Per Item User Rating Prediction Cost Function Multiple Users](/PerItemUserRatingPredictionCostFunctionMultipleUsers.PNG 'Cost function for per item user rating prediction for multiple users')

## Collaborative Filtering Algorithm

![Collaborative Filtering Cost Function](/CollaborativeFilteringCostFunction.PNG 'Cost function for collaborative filtering')

![Collaborative Filtering Gradient Descent](/CollaborativeFilteringGradientDescent.PNG 'Gradient descent for collaborative filtering')

## Collaborative Filtering With Binary Labels

![Collaborative Filtering Binary Labels Examples](/CollaborativeFilteringBinaryLabelsExamples.PNG 'Examples of applications of collaborative filtering with binary labels')

![Collaborative Filtering Binary Labels Cost Function](/CollaborativeFilteringBinaryLabelsCostFunction.PNG 'Cost function for collaborative filtering with binary labels')

## Mean Normalization

![Users Without Ratings](/MeanNormalizationUsersWithoutRatings.PNG 'Users without ratings')

In the above Eve has not rated any movies, so the parameters learned for her will be zero since they don't contribute anything to the model. To help fix this problem mean normalization is used.

![Mean Normalization](/MeanNormalization.PNG 'Mean normalization')

In the above the mean rating for each movie is calculated and then subtracted from the rating each user gave to the movie. The parameters $w$, $b$ and $x$ are learned and the model prediction for a movie is the application of the parameters plus the mean rating for the movie. In this way users that have not rated a movie (despite having 0 for their parameters) will end up with the average rating for the movie. In the above example the mean was calculated for each movie (the rows), however, as an alternative the mean could have been calculated for each user (the columns). Which is best depends on the application. Mean normalization in this way gives more reasonable predictions and also tends to make the collobarative filtering algorithm run a bit faster.

## TensorFlow Implementation

TensorFlow can automatically calculate the partial derivatives of the cost function during gradient descent.

![TensorFlow Gradient Descent With Auto Diff](/TensorFlowGradDescWithAutoDiff.PNG 'TensorFlow implementation of gradient descent with Auto Diff')

Using Auto Diff in TensorFlow other optimization algorithms can be used instead of gradient descent such as the Adam algorithm.

Below is an implementation of collaborative filtering using TensorFlow and the Adam optimizer:

![Collaborative Filtering TensorFlow Implementation](/CollaborativeFilteringTensorFlowImpl.PNG 'Collaborative filtering implemented in TensorFlow')

The targets, $Ynorm$ are mean normalized.

**Note** the collateral filtering algorithm and cost function doesn't neatly fit into the dense layer or the other standard neural network layer types of TensorFlow. That's why we the cost function is specified and then Auto Diff is used to automatically calculate the partial derivatives.

## Finding Related Items

A set of features learned by collaborative filtering from a group of items will likely be difficult for a human to interpret. However, the values of those features do capture something about what the item is like. Therefore items with similar values for the features are likely to be similar to each other.

To determine how similar 2 items are calculate their squared distance. The smaller the distance the more similar the items are.

![Finding Related Items](/FindingRelatedItems.PNG 'Finding related items')

## Limitations of Collaborative Filtering

![Limitations Of Collaborative Filtering](/CollaborativeFilteringLimitations.PNG 'Collaborative filtering limitations')

## Collabirative Filtering Versus Content Based Filtering

- Collaborative Filtering
    - Recommends items based on similarity of user ratings for other items.
    - Does not require features for the user, just user ratings of items.
- Content Based Filtering
    - Recommends items based on features of the user and the item.
    - Requires having some features for the user.

## Content Based Filtering

![Content Based Filtering User And Item Features](/ContentBasedFilteringUserAndItemFeatures.PNG 'Content based filtering user and item features')

Above are some examples of user and item features. The vectors of user features and item features do not need to be the same size.

![Content Based Filtering Learning Goal](/ContentBasedFilteringLearningGoal.PNG 'Content based filtering learning goal')

The goal of the content based filtering learning algorithm is to find the vector $v^{(j)}_u$ (for users) and $v^{(i)}_m$ (for items) that when the dot product is taken will make good predictions about which items to recommend to which users. The vectors $v^{(j)}_u$ and $v^{(i)}_m$ must be the same size.

## Content Based Filtering Architecture

Deep learning is used for the content based filtering algorithm, where separate neural networks are created for users and items to learn the feature vectors $v^{(j)}_u$ and $v^{(i)}_m$.

![Content Based Filtering Architecture 1](/ContentBasedFilteringArch1.PNG 'Content based filtering architecture separate user and item networks')

The separate user and item networks are combined by the process of calculating the dot product of the user and item vectors learned by the networks. The error of this dot product is what is ultimately minimized. If the target is a binary value then the logistic function can be applied to the dot product.

![Content Based Filtering Architecture 2](/ContentBasedFilteringArch2.PNG 'Content based filtering architecture combining separate user and item networks')

Similar items can be found by measuring the distance between item vectors.

![Content Based Filtering Finding Similar Items](/ContentBasedFilteringFindingSimilarItems.PNG 'Content based filtering finding similar items')

**Note** a disadvantage of content based filtering, especially on large datasets, is that it can be computationally expensive.

## Content Based Filtering On Large Datasets

For large datasets it is not feasible to calculate recommendations based on the entire dataset. Instead a small subset of the data that are plausible candidates for recommendation is selected and then those candidates are ranked in order to present the user with the best possible recommendations.

![Content Based Filtering Retrieval](/ContentBasedFilteringRetrieval.PNG 'Content based filtering retrieval')

The list of plausible recommendation candidates is then run through the learned model to rank them based on the predictions of the model. The item vectors have already been computed then only the user vector needs to be calculated by the model.

![Content Based Filtering Ranking](/ContentBasedFilteringRanking.PNG 'Content based filtering ranking')

There is a possible trade off between the number of candidates retrieved and the speed at which the recommender system will operate. Do some experiments can help to analyze this trade off and determine a good number of candidates to retrieve.

![Content Based Filtering Retrieval Analysis](/ContentBasedFilteringRetrievalAnalysis.PNG 'Content based filtering retrieval analysis')

## TensorFlow Implementation Of Content Based Filtering

![Content Based Filtering TensorFlow Implementation](/ContentBasedFilteringTensorFlowImpl.PNG 'Content based filtering TensorFlow implementation')

The above is an implementation of content based filtering using TensorFlow. It creates 2 neural networks, one for user features and the other of item features, both using the ReLu activation function and both having outputs of size 32 numbers. Both the user and item output vectors are normalized (l2 norm) to have size 1 because it turns out that doing so makes the algorithm work better. The dot product of the user and item vectors is calculated using a the special Keras layer Dot. The cost function is defined as mean square error.






