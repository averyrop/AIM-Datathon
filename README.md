## Project Title: Roswell Park's DBBR Cancer Patient Survival Prediction

Team Members: Avery Roper

Link to AIM Datathon: https://www.kaggle.com/c/aimdatathon2020/leaderboard <br>
Link to Google Collab Notebook: https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing
### Contents

* [Problem](#problem)
* [Solution](#solution)
* [ML Pipeline](#ml-pipeline)
* [Data Management](#data-management)
   * [Data Preprocessing](#data-preprocessing)
      * [Dataframe combination](#Dataframe-combination)
      * [Dealing with null values](#Dealing-with-null-values)
      * [Recombining test and train](#Recombining-test-and-train)
* [Study Design](#Study-Design) 
   * [Survival proportions](#Survival-proportions)
   * [Race proportions](#Race-proportions)
   * [Age spread](#Age-spread)
   * [Income spread](#Income-spread)
* [Train and Test Data Pre-processing](#Train-and-Test-Data-Pre-processing)
   * [Encode](#Encode)
   * [Scale](#Scale)
   * [Homogeneity check](#homogeneity-check)
   * [Separate training data](#Separate-training-data)
* [Test-train-Validation_Strategies](#Test-Train-Validation-Strategies)
* [Model Training and Tuning](#Model-Training_and_Tuning)
   * [Decision Tree Model Training](#Decision-Tree-Model-Training)
      * [Appendix (Previous decision tree models)](#Appendix-(Previous-decision-tree-models))
   * [Random Forest Model Training](#Random-Forest-Model-Training)
* [Results,Model Performance,Interpretability](#Results_Model-Performance_and_Interpretability)
   * [Decision Tree Results](#Decision-Tree-Results)
   * [Random Forest Results](#Random-Forest-Results)
   * [Feature Importance](#Feature-Importance)
      
* [Conclusion](#Conclusion)
* [Solution Video](#Solution-Video)

Each of the headers that correspond to a header in the google collab link to the header in the colab, for reference.


#### Problem
-  Provided with the dataset from the Roswell Park DataBank and BioRepository Shared Resource, we teams were tasked with predicting patient survival outcomes.

#### Solution
- In the initial attempt during the event, decision trees, subset selection, and boosting were used to attempt to predict the patient outcomes in R, and to identify important features in this prediction. Looking back on it the attempt was amateurish, so a revised version of the modeling was done in python using cross validated recursive feature elimination and random forests. The updated version achieves a much higher accuracy and about 500 important variables.

#### ML-Pipeline

![Pipe image](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/MLpipeline%20(2).png)

Link to Google Colab: https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing

#### [Data-Management](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=mMZZXXuhVyok)


##### [Data Preprocessing](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=BMc0gDb7uJ9A)

Revised model preprocessing.

##### [Dataframe Combination](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=oS17yqw_AGrr)

 Change all columns to uppercase 
 Merge all columns. 
 Consolidate test and training columns into unified columns, so they're identified as the same variable.

##### [Dealing with null values](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=RtuW8MzSAOGK)

 Eliminate columns with high ratio of blank responses
 Fill columns with low ratios of blank repsonses with most frequent response (as the unanswered columns were categorical)
      
##### [Recombining test and train](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=2FH_U1nTCaBn)

Recombine columns separated during previous transformation steps


#### [Study-Design](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=2u5ybW9ROhRy)


The clinical goal of this analysis was prediction of cancer patient survival. In the pursuit of this, the nearly 1200 pieces of data surrounding every patient were provided. This is too many to explore all of, but an exploration of some of these variables will be performed here to show the design, biases, and implications of this study. Only the data with the classes associated are explored, as these are the samples that influence the creation of the model.
##### [Survival proportions](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=Nl7iYWBVGFqW)

#### Figure A ####

##### Survival Breakdown #####
![Survival image](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/Survival%20pie.png)

The first variable explored is the patient survival itself. As can be seen in Figure A, 60% of the patients survived, meaning that if there are no other factors interacting, when exploring other variables, We should see that same 60:40 split when splitting the variable based on survival

##### [Race proportions](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=Vluyqb5PGV4o)

#### Figure B ####

##### Age Breakdown #####
![Age image](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/new%20age%20plot.png)

Looking at Figure B, we can see the distribution of ages used in the study. The ages are normally distributed but upon inspection it appears that there are two separate normal distribtions. One at about 60 for the survivors (blue) one at 70 for the dead (red). This is indicative of age being useful in predicting the patient status.

##### [Age spread](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=UBlvBllxGbRP)

#### Figure C ####

##### Race Breakdown #####
![Race image](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/Race%20pie.png)

This image shows the races used in this study. Race 1 is the stand in for white, and race 2 is the stand in for black. The others are translated in the google collab, but aren't really  necessary here to point out that a vast majority of the patients used in this study were white. As a result the results are not necessarily applicable to patients of different races. We also won't really be able to tell if race helps predict the survival status, as theres not enough data about other races to make an informed decision.

##### [Income spread](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=pFhM-8VjGeeX)

#### Figure D ####

##### Income Breakdown #####
![Income image](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/Income%20hist.png)

The last variable shows annual incomes of patients split based on patient survival. The phenomenon seen in the age histogram is not seen here. For patients who make under 25k per year, the red line is higher than or even with the the blue line, while with the patients who make above 25k the blue line is much higher. This suggests that income has some relation to the survival status.

#### [Train and Test Data Pre-processing](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=nLiBxFLfv_9W)

#####   [Encode](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=4JbAFQKmY0wQ)

Encode non numerical columns
#####   [Scale](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=9nXmFsqGY3az)

Scale all columns, now numerical (subtract mean and divide by standard deviation)

#####  [Homogeneity check](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=o7IihDpRZZmm)

Eliminate homogeneous columns (too many of the same answer)

#####   [Separate training data](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=AeD4_3Z4ayIV)

Separate class associated data for use in modelling


#### [Test-Train-Validation-Strategies ](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=uu4SxE7ZDUW3)

Recursive feature elimination with cross validation (RFECV) was used to tune the model. While this may seem to be better suited to be in the model tuning section, it is also related to the test train split. The entire class associated set of 1600 patients were taken for the model. 80% of these were then chosen as a training set (1280). This training set was then used with RFECV and random forests to eliminate unimportant columns from the dataframe fed in. RFECV used cross validation to split the training set into 5 subsets. A model was trained using 4 of the subsets in a random forest and was then checked for accuracy on the 5th subset. This was repeated until every subset got a turn to be the accuracy check set, and then all of the accuracies were averaged. This whole process was then repeated after eliminating 25 columns that were deemed unimportant. This process started at about 1000 columns and was then repeated until about 500 columns remained, all deemed to be important. This was based on the subsequent accuracies of the cross validations reaching an optimal number. 

So to summarize there are 5 different cross validating sub-training sets, followed by a training set of 1280 used to train the model, followed by an overall training set of 1600, which are the patients which our team was provided class labels for. Similarly there are also 5 cross validating test subsets, followed by a test set of 320 to test the model on, followed by the overall test set of 400 un class labeled patients, which are used to score the kaggle submission in the end.

#### [Model-Training_and_Tuning](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=MT5GcIpH4GdA)


##### [Decision Tree Model Training](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=WZtUWKdC5LrF)

A decision tree was grown and pruned using the training data. 

#### Figure E ####

![Python unpruned](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/unpruned%20python%20tree.png)

Figure E shows the unpruned tree. There are too many branches to try to make any sense of it, and it wouldn't be prudent to try as in it's current state it overfits the data

#### Figure F ####

![python cp](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/aim%20python%20cp%20%20pruning.png)

Cp values are plotted against accuracies in figure F in order to see how many branches we should trim in order to receive as high accuracy as we can while also making the plot as readable as possible. Higher cps mean simpler models

#### Figure G ####

![Python pruned](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/pruned%20aim%20tree.png)

Figure G shows the new pruned model. It is a lot more manageable than the unpruned version. The variables deemed important are: 
'BETA-CRYPTOXANTHIN','XDAYS45','ACTLDAYS','VB12DOS','BBPSYRES','DSODASV','AGESTART','AGEQUIT','BRND1TAR','BRND2FIL','MILKC','DOBYEAR','HHINC','SEERSUMMSTAGE2000_x'.

While some of the variables were lost in translation (can be translated yourself by looking through the DBBR survey file in the data) they mostly correspond to:
'Days per week of exercise at 45 years', 'Days per week taking acetylsalicylic acid', 'Had abnormal breast biopsy results', 'Diet soda serving size', 'Age first smoked', 'Age last smoked', 'Brand smoked most recently tar', 'Prior brand smoked most filter', 'How often did you eat milk on cereal', 'Year of Birth', 'Household income

###### [Appendix (Previous decision tree models)](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=3EgIPqBQzCVC)

In this section I go through some of the work I did in the original version of this project. It was originally done in R, and was riddled with mistakes, but for posterity the decsion trees made back then are shown here.

#### Figure H ####

![unpruned r](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/aim%20r%20unpruned.png)

Figure H shows unpruned tree from all of the variables

#### Figure I ####

![cp pruning r 1](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/aim%20r%20cp%20pruning.png)

Figure I shows the cp values in a slightly different format than before, but still used for pruning in the same way

#### Figure J ####

![R pruned](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/aim%20r%20pruned.png)

Figure J Shows the pruned tree. This tree also decided which variables it thought were most important.

#### Figure K ####

![var importance](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/r%20aim%20importances.png)

Figure K shows the variables deemed important by the pruned tree

#### Figure L ####

![imp plot](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/r%20aim%20importances%20plot.png)

Figure L shows relative importance of the previous variables to show just how much more important some are then others

#### Figure M ####

![unprune revised](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/unpruned%20revised%20r%20aim.png)

Figure M shows a new unpruned tree made using only the top most important variables gleaned from last model

#### Figure N ####

![cp selection 2](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/r%20cp%20pruning%20aim.png)

Figure N shows cp values for pruning of this tree

#### Figure O ####

![Revised tree 2](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/r%20pruned%20aim%20revised.png)

Figure O shows the new pruned tree


      
##### [Random Forest Model Training](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=_bVK08J25T6J)

A random forest model was used for the patient status predictions. This random forest used 200 trees with a max depth of 3. While this was a somewhat lower amount than I might usually use, the accuracy presented at the end was good enough that I feel it was okay, since increasing the number of trees and the depth would have increased the amount of time to wait for the results.

Here I will also discuss the strategy revision from the initial version of team 5's project from November to now. Random forest was chosen instead of boosting somewhat on a whim. They both have comparable accuracy, but the boosting builds on itself to better predict the harder samples that previous trees in the ensemble couldn't predict. In comparison the random forest just randomly uses a subset of features in each tree in the ensemble to predict the outcome. The decision was made based on 

1. The fact that I was subconciously afraid that the boosting model might overfit just a bit more than the random forest would, even though they're robust against overfitting
2. The fact that I recently used boosting in a different project and wanted to showcase a different technique here.
3. I felt that a random selection of features might give a more unbiased feature importance than seeking out features that are useful in predicting hard patients. I guess this is what I mean by the overfitting in 1.

Since they both have similar accuracy I figured that these reasons were good enough to choose one over the other.

#### [Results_Model-Performance_and_Interpretability](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=9Avhzg93Dxvg)


##### [Decision Tree Results](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=LMOhPjli4gWr)

The pruned tree is used here to predict the test set. the ROC AUC score achieved from this is .74. The following visualizations reinforce this.

#### Figure P ####

##### Confusion Matrix #####
Confusion     | Matrix
------------- | -------------
   64         |      54
   40         |     162
   
Figure P is a confusion matrix. Top left number is how many patients were accurately predicted as dead, and bottom left is the same for alive. More numbers in these positions are good.

#### Figure Q ####

![decision tree ROC](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/decision%20tree%20aim%20ROC%20curve.png)

Figure Q is the ROC curve for the decision tree .The roc curve shows the proportion of true positives to false positives. This is best when hugging the left wall and ceiling. As it stands the false positive rate increases with the true positive rate increases faster than the false until about .67 true positive then false positive starts increasing faster, somewhat negating any further increases in true positive. this makes the .67 somewhat optimal

##### [Random Forest Results](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=e6ZYz7Hl4xAC)

The results of RFECV random forest model were tested on set aside test set of 320 patients. The ROC_AUC score, the ROC curve, and a confusion matrix will be shown to illustrate the results. For interpretability, the variables/ questions associated with them that were selected by the feature selection will be summarized. A somewhat fuller account can be seen by visiting the colab, link mentioned earlier.

The internal ROC AUC score (Calculated based on my set aside test set of 320) was .76. 

The AUC score is related to the numbers of true and false positives and negatives in the predicted data. The confusion matrix looks like this:


#### Figure R ####

##### Confusion Matrix #####
Confusion     | Matrix
------------- | -------------
   42         |      76
   14         |     188
   

More numbers along the diagonal is good, and this is another way to look at/ interpret the ROC AUC score given above.
The actual ROC curve is shown here

#### Figure S ####

##### ROC curve #####
![ROC image](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/datathon%20ROC%20curve.png)

The closer this curve is to hugging the left wall and ceiling, the better the score is, while appearing as a diagonal line from bottom left to top right is bad.


##### [Feature Importance](https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing#scrollTo=Ur-mgVWK2QKv)
   
   
#### Figure S ####

![Feature elim](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/Feature%20elim%20aim%20plot.png)

Figure S shows the accuracies yielded by each feature subset gone through by RFECV, to illustrate how it chose 460 features. The general trend can be seen to increase until around 18 rounds of feature elimination (around 450 features eliminated, since 25 features are eliminated in each round) then starts to decrease a bit. If I were eliminating features by hand I'd have probably eliminated more as the general trend does not drop by that much, to achieve the very highest accuracy possible it is okay for the model to have just stopped at the highest accuracy

The questions used to diagnose the survival can be grouped into the following categiries:

1. General Nutrition (e.g. Fruits and vegetables per week, fried food frequency)
2. Smoking (e.g. Cigarettes per day, brand smoked)
3. Medications (ibuprofen, antacids)
4. Misc (e.g. age diagnosed, lithocode, workout, income)

While this is a very broad approximation, there were around 230 features deemed important, so I can't summarize them all here, but the google colab can be referenced for a more thorough viewing.


#### Conclusion

In conclusion, the final accuracy of this redone late submission on the public leaderbord was .75703. This can not be verified anywhere as it's not recorded since it was late, but I have nothing to gain from lying about it.

#### Solution-Video

Roswell Park's DBBR Cancer Patient Survival Prediction Video Link: [here](https://www.youtube.com/watch?v=dQw4w9WgXcQ&ab_channel=RickAstleyVEVO)
