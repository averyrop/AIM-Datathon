# AIM-Datathon
Summary of redone work done after AIM Datathon Nov. 2020

## Project Title: Roswell Park's DBBR Cancer Patient Survival Prediction Team #5

Team Members: Avery Roper and Ophelia Morey.

Link to AIM Datathon: https://www.kaggle.com/c/aimdatathon2020/leaderboard <br>
Link to Google Collab Notebook: https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing
### Contents

* [Problem]
* [Solution]
* [ML Pipeline]
* [Data Management]
* [Study Design]/ Exploratory Analysis 
* [Validation Strategies (Train and Test Data Pre-processing, Training/Validation Split)]
* Feature Engineering 
* [Model Training,Tuning (Random Forest/ RFECV)]
* [Results,Model Performance,Interpretability]
* [Conclusion]
* [Solution Video]
  * Link a short video (unlisted Youtube link) that walks through your approach from github and code from google collab.
* [Acknowledgments]

#### Problem
-  Provided with the dataset from the Roswell Park DataBank and BioRepository Shared Resource, we teams were tasked with predicting patient survival outcomes.

#### Solution
- In the initial attempt during the event, decision trees, subset selection, and boosting were used to attempt to predict the patient outcomes in R, and to identify important features in this prediction. Looking back on it the attempt was amateurish, so a revised version of the modeling was done in python using cross validated recursive feature elimination and random forests. The updated version achieves a much higher accuracy and about 500 important variables.

#### ML-Pipeline

- The ML workflow used in the revised model is as follows:
1. Import all data
2. Combine all data into one dataframe
3. Eliminate columns of responses that are unanswered by a majority of people
4. For columns answered by most people, fill unanswered rows with most common answer, so model can read data
5. Encode non- numerical answers to be readable by computer
6. Scale all columns/ responses to be readable by computer
7. Remove columns that are dominated by one answer, since common answer won't provide variety for prediction  (e.g. if 99% of respondents say they are male)
8. Split all data with patient survival provided into test and training
9. Train recursive cross validated feature elimination (RFECV) algorithm on training data, using random forest with 200 trees to predict outcome
10. Test RFECV model on test set
11. Use RFECV model to predict data where class is not provided
12. Save predictions as a submission file
13. Identify important variables based on those used in RFECV model

Link to Google Colab: https://colab.research.google.com/drive/1U9w9gz5ANz1qX-a6mlZQuvpBReR_zXuk?usp=sharing

#### Data-Management

Revised model preprocessing.

1. Change all columns to uppercase 
2. Merge all columns. 
3. Consolidate test and training columns into unified columns, so they're identified as the same variable.
4. Eliminate columns with high ratio of blank responses
5. Fill columns with low ratios of blank repsonses with most frequent response (as the unanswered columns were categorical)
6. Encode non numerical columns
7. Scale all columns, now numerical (subtract mean and divide by standard deviation)
8. Eliminate homogeneous columns (too many of the same answer)

#### Study-Design/ Exploratory Analysis
The clinical goal of this analysis was prediction of cancer patient survival. In the pursuit of this, the nearly 1200 pieces of data surrounding every patient were provided. This is too many to explore all of, but an exploration of some of these variables will be performed here to show the design, biases, and implications of this study. Only the data with the classes associated are explored, as these are the samples that influence the creation of the model.

The first variable explored is the patient survival itself. As can be seen in the pie chart below, 60% of the patients survived, meaning that if there are no other factors interacting, when exploring other variables, We should see that same 60:40 split when splitting the variable based on survival

![Survival image](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/Survival%20pie.png)

The next variable explored is age. The 60:40 split can be seen here between the red and the blue, as the blue bars look basically like the red bars, just about 1.5x times taller. This shows that age is not visually noticeable for the most part as indicator of patient survival. It also shows that an approximately normal distribution of patient ages were acquired with a mean of about 62.

![Age image](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/Age%20hist.png)

This image shows the races used in this study. Race 1 is the stand in for white, and race 2 is the stand in for black. The others are translated in the google collab, but aren't really  necessary here to point out that a vast majority of the patients used in this study were white. As a result the results are not necessarily applicable to patients of different races. We also won't really be able to tell if race helps predict the survival status, as theres not enough data about other races to make an informed decision.

![Race image](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/Race%20pie.png)

The last variable shows annual incomes of patients split based on patient survival. The phenomenon seen in the age histogram is not seen here. For patients who make under 25k per year, the red line is higher than or even with the the blue line, while with the patients who make above 25k the blue line is much higher. This suggests that income has some relation to the survival status.

![Income image](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/Income%20hist.png)



#### Validation-Strategies 

Recursive feature elimination with cross validation (RFECV) was used to tune the model. While this may seem to be better suited to be in the model tuning section, it is also related to the test train split. The entire class associated set of 1600 patients were taken for the model. 80% of these were then chosen as a training set (1280). This training set was then used with RFECV and random forests to eliminate unimportant columns from the dataframe fed in. RFECV used cross validation to split the training set into 5 subsets. A model was trained using 4 of the subsets in a random forest and was then checked for accuracy on the 5th subset. This was repeated until every subset got a turn to be the accuracy check set, and then all of the accuracies were averaged. This whole process was then repeated after eliminating 25 columns that were deemed unimportant. This process started at about 1000 columns and was then repeated until about 500 columns remained, all deemed to be important. This was based on the subsequent accuracies of the cross validations reaching an optimal number. 

So to summarize there are 5 different cross validating sub-training sets, followed by a training set of 1280 used to train the model, followed by an overall training set of 1600, which are the patients which our team was provided class labels for. Similarly there are also 5 cross validating test subsets, followed by a test set of 320 to test the model on, followed by the overall test set of 400 un class labeled patients, which are used to score the kaggle submission in the end.

#### Model-Training_and_Tuning

A random forest model was used for the patient status predictions. This random forest used 200 trees with a max depth of 3. While this was a somewhat lower amount than I might usually use, the accuracy presented at the end was good enough that I feel it was okay, since increasing the number of trees and the depth would have increased the amount of time to wait for the results.

Here I will also discuss the strategy revision from the initial version of team 5's project from November to now. Back in November A decision tree was used alongside boosting to predict the patient outcomes. During the initial attempt the school year was still going, so I was still experimenting with new methods. The decision tree was chosen for it's interpretability and the boosting was chosen for acurracy. best subset selection was also used, on the basis of a chi squared test. This time only a random forest was done alongside the RFECV explained in the last section. The decision tree was omitted as random forest typically has better accuracy. Random forest was chosen instead of boosting somewhat on a whim. They both have comparable accuracy, but the boosting builds on itself to better predict the harder samples that previous trees in the ensemble couldn't predict. In comparison the random forest just randomly uses a subset of features in each tree in the ensemble to predict the outcome. The decision was made based on 

1. The fact that I was subconciously afraid that the boosting model might overfit just a bit more than the random forest would, even though they're robust against overfitting
2. The fact that I recently used boosting in a different project and wanted to showcase a different technique here.
3. I felt that a random selection of features might give a more unbiased feature importance than seeking out features that are useful in predicting hard patients. I guess this is what I mean by the overfitting in 1.

Since they both have similar accuracy I figured that these reasons were good enough to choose one over the other.

#### Results_Model-Performance_and_Interpretability

The results of RFECV random forest model were tested on set aside test set of 320 patients. The ROC_AUC score, the ROC curve, and a confusion matrix will be shown to illustrate the results. For interpretability, the variables/ questions associated with them that were selected by the feature selection will be summarized. A somewhat fuller account can be seen by visiting the colab, link mentioned earlier.

The internal ROC AUC score (Calculated based on my set aside test set of 320) was .76. 

The AUC score is related to the numbers of true and false positives and negatives in the predicted data. The confusion matrix looks like this:

_____________
|  42  | 76  |
_____________
|  14  |188  |
_____________

More numbers along the diagonal is good, and this is another way to look at/ interpret the ROC AUC score given above.
The actual ROC curve is shown here

![ROC image](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/datathon%20ROC%20curve.png)

The closer this curve is to hugging the left wall and ceiling, the better the score is, while appearing as a diagonal line from bottom left to top right is bad.

The questions used to diagnose the survival can be grouped into the following categiries:

1. General Nutrition (e.g. Fruits and vegetables per week, fried food frequency)
2. Smoking (e.g. Cigarettes per day, brand smoked)
3. Medications (ibuprofen, antacids)
4. Misc (e.g. age diagnosed, lithocode, workout, income)

While this is a very broad approximation, there were around 230 features deemed important, so I can't summarize them all here. Again, the google collab can be referenced,
and the important features can be seen in the feature importance section


#### Conclusion

In conclusion, the final accuracy of this redone late submission on the public leaderbord was .75703. This can not be verified anywhere as it's not recorded since it was late, but I have nothing to gain from lying about it.

#### Solution-Video

[![Watch the video](https://github.com/aimsymposium/Project-sample/raw/main/video.png)](https://youtu.be/vOgCOoy_Bx0)


#### Acknowledgments
