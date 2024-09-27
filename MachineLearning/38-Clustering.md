# Clustering

Clustering algorithms automatically discover, within a dataset, groups of data points that are similar (in some way) to each other.

## K-Means Algorithm

Cluster centroid - the center of a cluster of data

Key steps:

1. Start by randomly selecting centroids (the number of which match the number of clusters desired).
2. For each data point determine which centroid it is closest to and assign it to that cluster.
3. For each cluster of data points, take the average of each data point in that cluster and change the location of the centroid for that cluster to the average.
4. Repeat steps 2 and 3 until there are no changes to the clusters that data points are assigned to and there are no changes to the location of the cluster centroids.

Algorithm:

1. Randomly initialize K cluster centroids
2. Repeat
   a. For each data point and each centroid calculate the distance between the data point and the centroid (the L2 norm) and square it, then assign the data point to the cluster where this value is a minimum.
   b. For each cluster, take the average of all the data points in the cluster and assign the new centroid to that average value.
3. Stop when no data points are assigned to a different cluster and when the centroids no longer change.

![K-Means Algorithm](/K-MeansAlgorithm.PNG 'K-means algorithm')

If a cluster ends up having no points assigned to it, then either:

1. The cluster can be eliminated (K becomes K - 1)
2. The centroids are reinitialized and the algorithm starts again with the new initial centroids.

In practice option 1 above is used more often.

## K-Means Optimization Objective

What the cost function that the K-means algorithm is trying to minimize is tha average squared distance between a point in a cluster and the centroid of that cluster.

![K-Means Cost Function](/K-MeansCostFunction.PNG 'K-means optimization objective')

Because the K-means algorithm is minimizing the cost function (average squared distance) it is guaranteed to converge.

**Note** the cost function can be used to determine when to stop the algorithm, namely when the value of the cost function stays the same or is going down very slowly.

## Initializing K-Means

Always choose the number of clusters to be less than the number of training examples.

The most common method for initializing the centroids for K-means is to randomly select K training examples to be the initial centroids.

It is possible for K-means to converge on a local optimum which is dependent on the initial centroids selected that is not as good, does not minimize the cost function as well, as some other local optimum (or the global optimum).

![K-Means Local Optima](/K-MeansLocalOptima.PNG 'K-means local optima')

In order to avoid this problem, K-means can be run many times with different intial centroids and the clustering that results in the lowest cost function can be selected as the best.

![K-Means Multiple Runs](/K-MeansMultipleRuns.PNG 'K-means multiple runs to find best local optima')

Runs K-means more than a 1000 times tends to be very comuputaionally expensive and results in diminishing returns.

## Choosing The Number Of Clusters

The value of K can be ambiguous when looking at the data.

One way to select the value of K is the _elbow method_, which is the run K-means for different values of K and see at which value the cost funciton starts to decrease significantly less.

![K-Means Elbow Method](/K-MeansElbowMethod.PNG 'K-means elbow method')

It is never a good idea to select the value of K that minimizes the cost function as higher values of K will always minimize the cost function.

Alternatively, K can be choosen based on the purpose of clustering the data and the trade offs inherent in that purpose for using fewer versus more clusters.

![K-Means Choosing K For Purpose](/K-MeansChoosingKForPurpose.PNG 'Choosing K based on the purpose for clustering')
