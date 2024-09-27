# Machine Learning Diagnositics

A test of a machine learning model to determine what is or is not working well in order to gain insight into how to improve the model.

## Model Evaluation

To evaluate a model, split the available data into a training set and a test set. A 70%/30% split of training data to test data is common, but so is a 80%/20% split.

![Model Evaluation Data Split](/ModelEvalDataSplit.PNG 'Model evaluation using split training and test data')

Then using the training data run the machine learning algorithm to find the parameters that minimize the cost function (over the training data). Calculate the average test error using the learned model. Also calculate the training error using the learned model.

![Model Evaluation Train And Test Error](/ModelEvalTrainAndTestError.PNG 'Model evaluation training and test errors')

Now compare the training error with the test error. A model that overfits (high variance) the training data will have a low training error but a high test error.

![Model Evaluation Apply Train and Test Error](/ModelEvalApplyTrainAndTestError.PNG 'Model evaluation applying training and test errors')

For classification problems, another way to calculate the training and test error is to calculate the fraction of the examples for each set that have been misclassified.

![Model Evaluation Alternative Test For Classification](/ModelEvalClassificationAltTest.PNG 'Model evaluation alternative test for classification')

## Better Model Evaluation (Cross Validation)

When evaluating different models just comparing each model using the test error is not a good practice because the parameter(s) that differentiate the models which may not be used in training are nonetheless parameter(s) that are modified to create the comparison. Therefore the test error likely underestimates the true generalization error.

![Model Selection Naive](/ModelSelectNaive.PNG 'Model selection naive approach')

To correct for this flaw, a better method is to split the data into 3 sets, training, cross validation and test.

![Model Evaluation Train, Cross Validation And Test Sets](/ModelEvalTrainCVTestSets.PNG 'Model evaluation training, cross validation and test sets')

Then calculate the error using the cross validation set for each model, and pick the model with the lowest cross validation error. Now the test error on the selected model is more likely to estimate the generalization error.

![Model Evaluation Train, Cross Validation And Test Errors](/ModelEvalTrainCVTestErrors.PNG 'Model evaluation training, cross validation and test errors')

![Model Selection Better](/ModelSelectBetter.PNG 'Model selection better approach')

In the above the parameter d or the degree of the polynomial is chosen based on the cross validation error and not the test error. Since no decisions on any parameter were made using the test error, it will be a good estimate for the generalization error (since it had no influence on any of the parameters),
