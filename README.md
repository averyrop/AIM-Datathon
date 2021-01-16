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

The last variable shows annual incomes of patients split based on patient survival. The phenomenon seen in the age histogram is not seen here. For patients who make under 50k per year, the red line is higher than or even with the the blue line, while with the patients who make above 50k the blue line is much higher. This suggests that income has some relation to the survival status.

![Income image](https://github.com/averyrop/Roswell-Park-s-DBBR-Cancer-Patient-Survival-Prediction-/blob/main/Income%20hist.png)



#### Validation-Strategies 
Refer to Train and Test Data Pre-processing, Training/Validation Split subtitles in the [Google Collab notebook](https://colab.research.google.com/drive/1GFtlNPVoSZ1RHcb2DvUzaLY8mEgdqeAV?usp=sharing) for more detail. 

-Train and Test Data Pre-processing
1) Encoding Categorical COlums. 2) Fill NaNs. 3) Drop columns.

-Training/Validation Split
80 % - 20 %  Split was used.

#### Model-Training_and_Tuning
Refer to Logistic Regression and XGBoost subtitles in the [Google Collab notebook](https://colab.research.google.com/drive/1GFtlNPVoSZ1RHcb2DvUzaLY8mEgdqeAV?usp=sharing) for more detail. 
Hyperparameters
Tuning

#### Results_Model-Performance_and_Interpretability
Refer to Results,.... subtitles in the [Google Collab notebook](https://colab.research.google.com/drive/1GFtlNPVoSZ1RHcb2DvUzaLY8mEgdqeAV?usp=sharing) for more detail. 

-Include Summary of Results/Discussion and Picture of Results here.


-Logistic Regression

![Image](https://github.com/aimsymposium/Project-sample/blob/main/LogisiticRegression.PNG)

A
B
C
D

-XGBoost

![Image](https://github.com/aimsymposium/Project-sample/blob/main/XGBoost.PNG)

A
B
C
D

#### Conclusion

XGBoost accuracy on test dataset was 76.9%.

#### Solution-Video

[![Watch the video](https://github.com/aimsymposium/Project-sample/raw/main/video.png)](https://youtu.be/vOgCOoy_Bx0)


#### Acknowledgments
