# Machine Learning Development Process

![Machine Learning Dev Cycle](/MachineLearningDevCycle.PNG 'Machine learning development cycle')

## Error Analysis

Select a random sample (100 to 200) of the examples the model has made an error on and directly examine those samples to see where the model went wrong and try to categorize the errors. From this analysis it will be easier to see what the most common type of errors are and how the model might be improved to do better. This type of analysis works well for problem domains where humans can easily recognize where the model goes wrong.

## Adding More Data

- Add more data of every type of example, e.g. "honeypot" project - might help, but can be expensive and time consuming.
- Add more data of the types indicated by error analysis that might help - often more cost effective.

## Data Augmentation

Modifying existing data to create new training examples.

![Data Augmentation](/DataAugmentation.PNG 'Data augmentation')

Distortion can be used to modify existing data, but care should be taken to select distortions that are relevant to the problem domain and not just introduce random noise.

![Data Augmentation Distortion](/DataAugmentationDistortion.PNG 'Data augmentation using distortion')

## Data Synthesis

Using artificial data inputs to create new data the is sufficiently like the real data a model should be trained on.

Below is an example of real data collected from a street scene where the model needs to be able to read the text in the scene. Next to the real data are some fonts available on most computers that in combination with varying the text and background color can be used to create synthetic data that resembles the real data.

![Data Synthesis Real Data](/DataSynthesis1.PNG 'Real data shown next to some data that can be used for synthesis')

![Data Sysnthesis Synthetic Data](/DataSynthesis2.PNG 'Real data next to synthetic data')

## Model-Centric Versus Data-Centric Approaches

In the past machine learning researches used fixed data sets and concentrated on refining the algorithms that were trained on them, a model-centric approach. Now many of these algorithms have matured and work well on diverse problem domains. Therefore, often a data-centric approache is taken where the data is engineered (through data augmentation or data synthesis) to create more data of a type that more narrowly defines a problem domain in order to get a model better tailored to it.

## Transfer Learning

Using data from one task to pre-train a model that is then trained again, fine tuned, on a smaller and different (but similar type) data set for a different task. What the pre-trained model learned is thereby 'transfered' to the new fined tune model to solve a different task.

![Transfer Learning](/TransferLearning.PNG 'Transfer learning')

In the above a pre-trained model is then fine tuned by removing the ouput layer and replacing it with one that is suited to the new task. Then either can train the parameters of only the output layer (works well if you only have a small data set) or can train the entire network with the parameters intialized to the parameters of the pre-trained model (works well if you have a larger data set).

![Transfer Learning Why Works](/TransferLearningWhyWorks.PNG 'Why transfer learning works')

Transfer learning works because, so long as the input is of a similar type, the hidden layers of the pre-trained model 'learn' to detect features that are useful beyone the task the model was originally trained for.

Pre-trained networks are available freely to be downloaded for fine tuning. Fine tuning involves using much less data that was used for the pre-trained model. This saves a lot of time and expense.

## Skewed Datasets

A dataset is skewed if there is a big difference between the number of positive and negative examples in the training data. For example, a dataset on a rare disease may have very few cases of patients with the disease. This means that different error metrics need to be used in order to get an accurate measure of how well a model trained on the skewed dataset is preforming.

For binary classificaiton problems, a confusion matrix, which classifies the predictions of the model based on the actual target values (from the cross validation set), is a useful tool for evaluating models trained on skewed datasets.

![Precision And Recall](/PrecisionAndRecall.PNG 'Precision and recall')

In the above two metrics computed from the confusion matrix that are useful for evaluating a model are:

- Precision - true positives / predicted positives - or the probability that a predicted positive is an actual positive
- Recall - true positives / actual positives - or the probability that an actual positive will be predicted by the model

Unfortunately there is a trade off between precision and recall.

For logistic regression setting the threshold for predicting positive values:

- higher - higher precission but lower recall
- lower - lower precision but higher recall

![Precision And Recall Trade Off](/PrecisionAndRecallTradeOff.PNG 'Precision and recall trade off')

Because of this trade off and the fact that precision and recall are separate metrics, another metric, the F1 score, is often used to decide which model to choose. Beacuse a low precision or low recall is often not good, the F1 score is designed to take that into account.

![F1 Score](/F1Score.PNG 'F1 score')

## Full Cycle Of A Machine Learning Project

![Full Cycle Machine Learning Project](/FullCycleMLProject.PNG 'Full cycle of a machine learning project')

![Machine Learning Project Deployment](/MLProjectDeployment.PNG 'Deployment of a machine learning project')

## Ethics

It is our ethical responsibility to do our best to ensure that machine learning applications are reasonably free from harmful bias and are not deployed for harmful purposes.

Some guidelines are:

![Ethical Guidelines](/EthicalGuidelines.PNG 'Ethical guidleines')
