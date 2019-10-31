# A Comprehensive Guide to Ensemble Learning

## Basic Introduction

A diverse group of people are likely to make better decisions as compared to individuals. Similar is true for a diverse set of models in comparison to single models. This diversification in machine learning is achieved by a technique called **Ensemble Learning**.

## Simple Ensemble Techniques

### Max Voting

The max voting method is generally used for classification problems. In this technique, multiple models are used to make predictions for each data point. The predictions by each model are considered as a 'vote'. The predictions which we get from the majority of the models are used as the final prediction.

Max voting can be implemented with [sklearn.ensemble.VotingClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.VotingClassifier.html).

### Averaging

Similar to max voting technique, multiple predictions are made for each data point in averaging. Averaging can be used for making predictions in regression problems or while calculating probabilities for classification problems.

### Weighted Averaging

This is an extension of the averaging method. All models are assigned different weights defining the importance of each model for prediction. Make sure the sum of all weights equals to 1.

## Advanced Ensemble Techniques

### Stacking

Stacking is an ensemble learning technique that **uses predictions from multiple models to build a new model**.

Below is a stepwise explanation for a simple stacking ensemble:

1. The train set is split into 10 parts.
2. A base model is fitted on 9 parts and predictions are made for 10th part. This is done for each part of the train set.
3. The base model is then fitted on the whole train set.
4. Using this model, predictions are made on the test set.
5. Steps 2 to 4 are repeated for another base models, resulting in another set of predictions for the train set and test set.
6. The predictions from the train set are used as features to build a new model.
7. This new model is used to make final predictions on the test set.

### Bagging

The idea behind bagging is combining the results of multiple models to get a generalized result. If all models are created on the same set of data and combine it, there is a high chance that it won't be useful. Bootstrapping can be used to solve this problem.

Bootstrapping is a sampling technique in which we create subsets of observations from the original dataset, [with replacement](https://en.wikipedia.org/wiki/Sampling_(statistics)#Replacement_of_selected_units). The size of the subsets is the same as the size of the original set.

Bagging (or Bootstrapping Aggregating) technique uses these subsets (bags) to get a fair idea of the distribution (complete set). The size of subsets created for bagging may be less than the original set.

Below is a stepwise explanation for bagging ensemble:

1. Multiple subsets are created from the original dataset, selecting observations with replacement.
2. A base model (weak model) is created on each of these subsets.
3. The models run in parallel and are independent of each other.
4. The final predictions are determined by combining the predictions from all models.

### Boosting

Boosting is a sequential process, where each subsequent model attempts to correct the errors of the previous model. The succeeding models are dependent on the previous model.

Below is a stepwise explanation for boosting ensemble:

1. A subset is created from the original dataset.
2. Initially, all data points are given equal weights.
3. A base model is created on this subset.
4. This model is used to make predictions on the whole dataset.
5. Errors are calculated using the actual values and predicted values.
6. The observations which are incorrectly predicted, are given higher weights.
7. Another model is created and predictions are made on the dataset.
8. Similarly, multiple models are created, each correcting the errors of the previous model.
9. The final model (strong learner) is the weighted mean of all the models (weak learners).

## Algorithms based on Bagging and Boosting

### Bagging Algorithms

- [Bagging meta-estimator](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.BaggingClassifier.html#sklearn.ensemble.BaggingClassifier)
- [Random forest](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html#sklearn.ensemble.RandomForestClassifier)

### Boosting Algorithms

- [AdaBoost](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.AdaBoostClassifier.html#sklearn.ensemble.AdaBoostClassifier)
- [Gradient Boosting](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingClassifier.html#sklearn.ensemble.GradientBoostingClassifier)
- [XGBoost](https://xgboost.readthedocs.io/en/latest/)
- [LightGBM](https://lightgbm.readthedocs.io/en/latest/)
