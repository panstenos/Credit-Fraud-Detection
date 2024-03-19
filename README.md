# Credit-Fraud-Detection
Using ML techniques to detect fraudulent transactions in a highly imbalanced dataset

## Context
Credit card fraud poses a significant threat to both financial institutions and cardholders worldwide. With the rise of online transactions and digital payments, fraudulent activities have become increasingly sophisticated, costing billions of dollars each year. In response, financial institutions and regulatory bodies are leveraging advanced technologies to detect and prevent fraudulent transactions in real-time.

The impact of credit card fraud extends beyond financial losses, affecting consumer trust, brand reputation, and regulatory compliance. For financial institutions, effectively detecting and mitigating fraud is essential to safeguarding their customers' assets and maintaining market credibility. Furthermore, addressing fraud proactively can lead to cost savings, improved customer satisfaction, and reduced legal liabilities.

To combat credit card fraud effectively, financial institutions employ advanced analytics, machine learning algorithms, and artificial intelligence technologies. These solutions analyze transactional data in real-time, identifying patterns, anomalies, and suspicious activities indicative of fraud. By leveraging historical transaction data, behavioral analytics, and predictive modeling, these systems can accurately detect fraudulent transactions while minimizing false positives.

## <u>About the Dataset</u>
The dataset is fairly large consisting of 6.4 million rows of data. There are 11 columns with the following values: 

| Field           | Description                                                                                           |
|-----------------|-------------------------------------------------------------------------------------------------------|
| step            | A unit of time in the real world, where 1 step represents 1 hour. Total steps in the simulation: 744  |
| type            | Type of transaction: CASH-IN, CASH-OUT, DEBIT, PAYMENT, TRANSFER                                      |
| amount          | Amount of the transaction in local currency                                                           |
| nameOrig        | Customer who initiated the transaction                                                                |
| oldbalanceOrg   | Initial balance before the transaction                                                                |
| newbalanceOrig  | New balance after the transaction                                                                     |
| nameDest        | Recipient of the transaction                                                                          |
| oldbalanceDest  | Initial balance of the recipient before the transaction                                               |
| newbalanceDest  | New balance of the recipient after the transaction                                                    |
| isFraud         | Indicates whether the transaction is fraudulent or not, the **target variable**                       |
| isFlaggedFraud  | Flags illegal attempts: An attempt to transfer more than $200,000 in a single transaction             |

The dataset was very imbalanced between the two classes with only 8213 rows of fraudulent transaction data, resulting in an imbalance of 1:773!
The figure below shows the percentage of each class present in each transaction type.

![image](https://github.com/panstenos/Credit-Fraud-Detection/assets/112823396/17448f62-43ad-4e28-b642-05fe0dfa23cb)

## <u>Choice of Metrics</u>
This dataset exhibits a common characteristic of credit fraud transactions: high class imbalance. Given this imbalance, relying on accuracy for evaluating model performance is not advisable. Instead, our primary objective should be to ensure that the model effectively identifies all instances of fraud. While allowing some false positives is acceptable, missing any fraudulent transactions can have significant consequences. Therefore, our baseline expectation is for the model to have high recall in detecting fraud, prioritizing sensitivity over precision.

Another metric that was used to evaluate the performance of the models was Cohen's Kappa. Cohen's Kappa is a statistical measure used to assess the level of agreement between two raters or classifiers. Cohen's Kappa is often used as an evaluation metric for classification tasks, especially when dealing with imbalanced datasets. This markdown discusses Cohen's Kappa and why it is preferred for imbalanced datasets. Cohen's Kappa is derived from the concept of observed agreement and expected agreement. It measures the agreement between the observed accuracy and the expected accuracy, correcting for the possibility of the agreement occurring by chance.

## <u>SMOTE and undesampling</u>
To achieve a good class balance for training and validation mainly 2 data sampling techniques were used: Synthetic Minority Oversampling (SMOTE) and majority Undersampling. In a few words, SMOTE is a technique that oversamples the minority class by artificially generating new data and at the same time undersamples the majority class data to achieve a desired class balance. Undersampling simply undersamples the majority class to achieve a desired class balance. 

For the cases where SMOTE was used, Undersampling of the majority class was first applied to reach a class balance of 1:2. Then the data were split into train and test and only then SMOTE was applied on the train data to achieve a class balance of 1:1. 
This action was taken to ensure that the validation set did not contain any synthetic data, which might have been more accurately predicted by the model, potentially leading to inflated validation results.

## <u>Model Performance Comparison</u>

Random Forrest classifier seemed to have the best perfomance in this dataset with validation precision exceeding 99% and a Cohen's Kappa exceeding 98%. Here is how the various Random Forrest models performed on the validation set:

![image](https://github.com/panstenos/Credit-Fraud-Detection/assets/112823396/4d3b1db6-ed19-4fc0-8617-60b7fe6060d3)

## <u>Feature Importance</u>

The features with the most importance on the Random Forrest Classifier were:

![image](https://github.com/panstenos/Credit-Fraud-Detection/assets/112823396/46880a4d-8180-4fb7-a5ae-433bc45c87b8)


For the last model, two new columns were created: 

'diffbalanceDest' = 'newbalanceDest' - 'oldbalanceDest',

'diffbalanceOrig = 'newbalanceOrig' - 'oldbalanceOrg'.

The new feature importances following this change were: 

![image](https://github.com/panstenos/Credit-Fraud-Detection/assets/112823396/a0aff5f8-54a9-46e0-af56-dcec94f39739)

