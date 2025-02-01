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

Looking into the count of the target value, we can see that the data for the two classes is very imbalanced. 88.7% of the clients responded 'no' and 11.3% responded yes. That's 36,548 no's and 4,640 yes's.

![class](https://github.com/user-attachments/assets/cdba4d95-3cf5-4321-84f4-0e3d2a652b05)

