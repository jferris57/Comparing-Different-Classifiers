# Comparing Different Classifiers
The goal of this project was to compare the performance of different classifiers (k-nearest neighbors, logistic regression, decision trees, and support vector machines) using a dataset related to the marketing of bank products over the telephone. This dataset comes from the UC Irvine Machine Learning repository and it can be found [here](https://archive.ics.uci.edu/dataset/222/bank+marketing) .

The data is related with direct marketing campaigns of a Portuguese banking institution. The marketing campaigns were based on phone calls. Often, more than one contact to the same client was required, in order to access if the product (bank term deposit) would be ('yes') or not ('no') subscribed. The classification goal is to predict if the client will subscribe to a term deposit (CD).

## Data Understanding
Additional information regarding the features and what they mean:

### Input variables:

### Bank client data:

1 - age (numeric)

2 - job : type of job (categorical: 'admin.','blue-collar','entrepreneur','housemaid','management','retired','self-employed','services','student','technician','unemployed','unknown')

3 - marital : marital status (categorical: 'divorced','married','single','unknown'; note: 'divorced' means divorced or widowed)

4 - education (categorical: 'basic.4y','basic.6y','basic.9y','high.school','illiterate','professional.course','university.degree','unknown')

5 - default: has credit in default? (categorical: 'no','yes','unknown')

6 - housing: has housing loan? (categorical: 'no','yes','unknown')

7 - loan: has personal loan? (categorical: 'no','yes','unknown')

### Related with the last contact of the current campaign:

8 - contact: contact communication type (categorical: 'cellular','telephone')

9 - month: last contact month of year (categorical: 'jan', 'feb', 'mar', ..., 'nov', 'dec')

10 - day_of_week: last contact day of the week (categorical: 'mon','tue','wed','thu','fri')

11 - duration: last contact duration, in seconds (numeric). Important note: this attribute highly affects the output target (e.g., if duration=0 then y='no'). Yet, the duration is not known before a call is performed. Also, after the end of the call y is obviously known. Thus, this input should only be included for benchmark purposes and should be discarded if the intention is to have a realistic predictive model.

### Other attributes:

12 - campaign: number of contacts performed during this campaign and for this client (numeric, includes last contact)

13 - pdays: number of days that passed by after the client was last contacted from a previous campaign (numeric; 999 means client was not previously contacted)

14 - previous: number of contacts performed before this campaign and for this client (numeric)

15 - poutcome: outcome of the previous marketing campaign (categorical: 'failure','nonexistent','success')

### Social and economic context attributes

16 - emp.var.rate: employment variation rate - quarterly indicator (numeric)

17 - cons.price.idx: consumer price index - monthly indicator (numeric)

18 - cons.conf.idx: consumer confidence index - monthly indicator (numeric)

19 - euribor3m: euribor 3 month rate - daily indicator (numeric)

20 - nr.employed: number of employees - quarterly indicator (numeric)

### Output variable (desired target):

21 - y - has the client subscribed a term deposit? (binary: 'yes','no')

After reading the description of the features, it says regarding the duration column that it "should be discarded if the intention is to have a realistic predictive model". I removed this column.

After initial examination of the data, I saw there were no missing values. I looked into the values of the 'default' column and noticed out of the entire dataset, only 3 people were confirmed to have credit in default. The rest did not or it was unknown. I don't believe this will be of much use to us so I removed it.

Looking into the count of the target value, it's clear that the classes are very imbalanced. 88.7% of the clients responded 'no' and 11.3% responded yes. That's 36,548 no's and 4,640 yes's.

![class](https://github.com/user-attachments/assets/cdba4d95-3cf5-4321-84f4-0e3d2a652b05)

We can see that most of the people that were contacted were in their 20's, 30's and 40's. Of those who did subscribe, most of them were in their late 20's and 30's.

![age](https://github.com/user-attachments/assets/9996ce2d-832b-48b9-9a45-76bd3e2c69d3)

![age-by-class](https://github.com/user-attachments/assets/865e7aab-5fc8-40f1-88f0-ed2f74856535)

![age-by-job](https://github.com/user-attachments/assets/db8df56d-c18d-4ce0-b6a2-0b5947aad5b8)

Those who subscribed mostly held administrative jobs followed by technicians and blue-collar workers.

![job-type](https://github.com/user-attachments/assets/22733f88-a9a1-4c7b-a91b-c998f566b2ff)

Most subscriptions came from married people, followed those who were single and divorced. However, the highest subscription rate came from those who were single at 14%.

![marital-status](https://github.com/user-attachments/assets/de9af364-4180-484f-b70d-2f8a7b841df7)

Married subscription %: 10.16

Single subscription %: 14.0

Divorced subscription %: 10.32

When filtering by education level, the highest amount of subscriptions came from those who hold a university degree.

![education](https://github.com/user-attachments/assets/e9883a5a-5747-4ade-8cff-2bdd1ca7024d)

Lastly, there was higher enrollment from the clients who were never contacted before or subscribed previously (previous successful campaign).

![previous-campaign](https://github.com/user-attachments/assets/74fa0a61-e310-49f7-af8b-4882be6fd05c)

I also plotted a correlation matrix of the numerical values as a heatmap. We can see high correlations between things like euribor 3 month rate and employment variation rate, consumer price index and employment variation rate.

![correlation](https://github.com/user-attachments/assets/d1882eb6-a670-4ad8-8ee2-ab8dcd8ad1ac)

## Understanding the Business Objective

The primary objective of this task is to develop a model that can accurately predict potential customers who will subscribe to a bank term deposit. The model will use data from bank clients (age, job, marital status, loans), previous interactions (outcome of a previous campaign), and social and economic factors. The business objective is to increase subscription rate for bank term deposits by identifying the most likely candidates.

I believe that given the fact we are trying to identify the most likely candidates, the bank wants to avoid spending time and resources on candidates who are not likely to subscribe. Because of this, I think maximizing precision and reducing false positives will be most important.

## Modeling

### Baseline Model

Before we build our first model, we want to establish a baseline. I chose to build a dummy classifier as our baseline model. The performance was not great:

Dummy Classifier Performance:

Accuracy: 0.7994658897790726

Precision: 0.10561056105610561

Recall: 0.10267379679144385

F1 Score: 0.10412147505422993

### Simple Models

I then trained a Logistic Regression model, KNN model, Decision Tree, and SVM model all with default parameters. I tracked the training time and training/test accuracy. Here were the results:

![image](https://github.com/user-attachments/assets/78858de6-a5ec-46e6-83ce-3e7b78e99238)

### Improving the Models

With some basic models trained, we want to improve them. I used GridSearch for hyperparameter tuning and to find the best parameters for our models.

Here are the param grids for each model:

![image](https://github.com/user-attachments/assets/ef8cdbc0-02d2-4e9f-bc1c-ce79feac8975)

![image](https://github.com/user-attachments/assets/2eefb287-6ccb-4bbc-b97e-844e623f8d24)

![image](https://github.com/user-attachments/assets/e68d2f5d-8874-44ff-953a-70295e73e88a)

![image](https://github.com/user-attachments/assets/36bfe0b0-5473-4039-a6c5-a0c6cb4013b8)














