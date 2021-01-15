# General Assembly Data Science Immersive Capstone Project: Inferring the number of open ion channels for the patch-clamp technique using machine learning.

## Introduction:
The aim of this project is to train machine learning models to deduce the number of open ion channels from synthetic signal data which is a good facsimile of the real electrophysiological signals.

This was first done using neural networks in Deep-Channel uses deep neural networks to detect single-molecule events from patch-clamp datahttps://doi.org/10.1038/s42003-019-0729-3 with the synthetic data being created using a semi synthetic method. The primary motivation for this paper is stated as endeavouring to improve the quality and capabilities of patch-clamp electrophysiology using computer modelling techniques as not only does the data need to be manually interpreted, but also there has been no improvement in the physical equipment over the past couple of decades.

The paper used a stochastic Markovian process to model the transition of the ion between open and closed states there by creating a signal which had noise from the data capturing process added by passing the modelled signal though the patch-clamp amplifier equipment. For some parts of the data a current "drift" was added. This procedure created a data set for which the true number of iron channels open at any point in time was known, which is both unachievable for real data and necessary for training machine learning models.

In an effort to get others help in devising methods to analysis the signal data a Kaggle competition, https://www.kaggle.com/c/liverpool-ion-switching/overview, was put up which used for its data source a combination of the ion channel models, sometimes with the results of two added together for a maximum of 10 open ion channels.

## Data Processing:

### Signal &quot;Drift&quot;:
Several of the batches which make up the data set had had a drift current added to them by means of a MATLAB script. The drift appears to have been added using parts of sine waves. This was removed by approximating it to a sine wave in this &quot;driftless&quot; data set which was downloaded from [https://www.kaggle.com/cdeotte/data-without-drift](https://www.kaggle.com/cdeotte/data-without-drift). This data set resulted in greatly improved model performance.

### Feature Engineering:
Three features were engineered from the data. The first of which was the exponential weighted mean, followed by the gradient of the signal and the gradient of the gradient.

## Exploratory Data Analysis:
During the feature engineering stage the change in the number open ion channels was computed as well as the length of time between the number of open ion channels changing.

#### Histogram of the Number of Open Ion Channels:
![Histogram of Number of Open Ion Channels](https://github.com/HindsonJF/Data-Science-Projects/blob/main/graphs_for_readme/Histogram_of_number_of_open_ion_channels.png)

#### Plot of the Number of Open Ion Channels Against Time:
![Lineplot of Number of Open Ion Channels](https://github.com/HindsonJF/Data-Science-Projects/blob/main/graphs_for_readme/lineplot_of_open_ion_channels_vs_time.png)

#### Histogram of the Change in Number of Open Ion Channels:
![Histogram of Diff\_Open\_Channels](https://github.com/HindsonJF/Data-Science-Projects/blob/main/graphs_for_readme/histogram_of_the_change_in_number_of_open_ion_channels.png)

#### Historgram of the Length of Time for which the Number of Ion Channels Open Remains the Same:
![Histogram of length\_time\_same (Logarithmic)](https://github.com/HindsonJF/Data-Science-Projects/blob/main/graphs_for_readme/Histogram_of_length_of_time_number_of_ion_channels_open_remains_same_log.png)

#### Correlation Heat Map:
![Correlation heat Map](https://github.com/HindsonJF/Data-Science-Projects/blob/main/graphs_for_readme/heat_map_corr.png)

## Modelling:
Eight models were fitted in total with three of them having F1-Macro scores of 0.83 on the holdout test data set. This suggests the models have been well fitted and that any further improvement would come from better signal processing or feature extraction.
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

### Baseline Prediction Model:
First the baseline model was established which always predicted the most frequent class. This was to predict that zero ion channels were open which was the case 24.8% of the time. The model had an F1-Macro Score of 0.04.

### Poisson Regressor Model:
Due to the signal values appearing to have been distributed like that of a Poisson distribution, a Poisson Regression was modified to give discrete outputs and fitted to the data. This model never predicted that zero ion channels would be open and so fail to predict the most common class. The model was modified to give integer outputs. This resulted in an F1-Macro Score of 0.18 indicating that this was a total unsuitable model.

### Logistic Regression Model:
The next model tried was a Logistic Regression. A grid search was run, with the best estimator possessing an F1-Macro Score of 0.54.

### K-Nearest-Neighbour Model:
A K-Nearest Neighbour model had a grid search run over the parameter space, which due to RAM demands had to be done in the cloud using Amazon Web Services EC2 platform, yielding a best estimator with an F1-Macro Score of 0.83

### Decision Tree Model:
A less computational demanding model, the Decision Tree had a grid search run over several parameters with the best estimator having an F1-Macro Score of 0.83.

### Random Forest Model:
A Random Forest model had a grid search run over a quite limited amount number of different parameters which tended towards the ensemble model being smaller due to the hardware not being sufficiently capable for larger ones. The best model had an F1-Macro Score of 0.83.

### Naive Bayes Models:
Two Naïve Bayes Models were fitted. The first a Gaussian Naïve Bayes model had an F1-Macro Score of 0.75 whereas the Bernoulli Naïve Bayes had an F1-Macro Score of 0.13.

## Conclusion:
It is possible to get reasonably accurate predictions for the number of open ion channels using primarily just the signal strength if the data has been cleaned by removing any drift manually. KNN, Decision Trees and Random Forests Models all performed roughly equally well suggesting that a limit was reach as to models could infer form the data it was trained with. Better signal processing and feature extraction should be able to lead to very high levels of accuracy, possibly with F1-Macro Scores over 95%.
It seems that with such improvements it will be the case that the interpreting of patch clamp data could be mostly automated, except for the removal of drift in the signal.

## Challenges and Key Lessons Learnt:
The main challenges were:
* Creating good features from the data.
* Fitting computationally demanding models on consumer hardware.
* Managing notebooks and code written to keep the work on the project coherent.

The key lessons learnt were:
*  Sometimes several potential solutions will need to be explored before finding one which works.
* On demand cloud computing can be very useful in running computationally intensive tasks for quite reasonable costs.
* More time should be spent researching any specialist techniques are packages for a specific type of problem before embarking on creating the data processing pipeline and running models.
