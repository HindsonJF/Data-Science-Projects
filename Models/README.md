# Modelling:
Eight models were fitted in total with three of them having F1-Macro scores of 0.83 on the holdout test data set. This is suggestive of the models having well fitted and that any further improvement would come from better signal processing or feature extraction.
| **Model** | **Macro F1 - Scores -->** | | | |
| --- | --- | --- | --- | --- |
| | **With Drift** | **With Drift** | **Without Drift** | **Without Drift** |
| | **Train** | **Test** | **Test** | **Submission to Kaggle** |
| Baseline | 0.04 | 0.04 | 0.04 | 0.076 |
| Rounded Poisson | 0.08 | 0.09 | 0.18 | 0.369 |
| Logistic Regression | 0.18 | 0.17 | 0.54 | 0.529 |
| K-Nearest Neighbour | 1.00 | 0.51 | 0.83 | 0.831 |
| Random Forests | 1.00 | 0.47 | 0.83 | 0.836 |
| Decision Tree | 0.54 | 0.52 | 0.83 | 0.832 |
| Gaussian Naïve Bayes | 0.21 | 0.21 | 0.75 | 0.753 |
| Bernoulli Naïve Bayes | 0.08 | 0.08 | 0.13 | 0.156 |

## Baseline Prediction Model:
First the baseline model was established which always predicted the most frequent class. This was to predict that zero ion channels were open which was the case 24.8% of the time. The model had an F1-Macro Score of 0.04.

## Poisson Regressor Model:
Due to the signal values appearing to have been distributed like that of a Poisson distribution, a Poisson Regression was modified to give discrete outputs and fitted to the data. This model never predicted that zero ion channels would be open and so fails to predict the most common class. The model was modified to give integer outputs. This resulted in an F1-Macro Score of 0.18 indicating that this was a total unsuitable model.

## Logistic Regression Model:
The next model tried was a Logistic Regression. A grid search was run, with the best estimator possessing an F1-Macro Score of 0.54.

## K-Nearest-Neighbour Model:
A K-Nearest Neighbour model had a grid search run over the parameter space, which due to RAM demands had to be done in the cloud using Amazon Web Services EC2 platform, yielding a best estimator with an F1-Macro Score of 0.83.

## Decision Tree Model:
A less computational demanding model, the Decision Tree had a grid search run over several parameters with the best estimator having an F1-Macro Score of 0.83.

## Random Forest Model:
A Random Forest model had a grid search run over a quite limited amount number of different parameters which tended towards the ensemble model being smaller due to the hardware not being sufficiently capable for larger ones. The best model had an F1-Macro Score of 0.83.

## Naïve Bayes Models:
Two Naïve Bayes Models were fitted. The first a Gaussian Naïve Bayes model had an F1-Macro Score of 0.75 whereas the Bernoulli Naïve Bayes had an F1-Macro Score of 0.13.
