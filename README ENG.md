# Credit Card Fraud Detection & Risk Optimization

The fraud detection and mitigation efforts on credit cards were conducted in a completely professional manner. A machine learning model is initiated with an imbalanced dataset resulting in 99% overall model accuracy, but more than likely it failed to detect any valid fraudulent transactions. I did not care about building a model to collect technical metrics. My only measure was the total dollar amount taken from the bank vault on the basis of fraudulent transactions.

My intention was not only to detect fraud transactions (which is what machine learning does), but to find a balance between the cost associated with blocking an innocent customer from using their card (as an annoying inconvenience) and allowing a fraudulent transaction through (which costs money).

The Extent of the Fraud Issue I Used the Kaggle Credit Card Fraud Dataset to conduct the Fraud Analysis. Only 492 out of 284,000 transactions were fraud-related, or Approximately 0.17% of All Transactions Were Fraud. If I wrote a Simple Function That Just Called All Transactions Safe, the Function Would Have A 99.8% Accuracy. This is the reason accuracy was never an element in Evaluating the Model, Since Day One.

Standard charts were not easily used to Display the Data, so I Had to Compress the Y-Axis Using Logarithmic Scale in Order to View the Minority Class (fraud) with Respect to the Total Quantity of Transactions (found no fraud). I conducted an extensive analysis of the Time and Amount Variables Before I Ever Began Building Models to Deconstruct the Classic Misconception of Transaction Amount.

# My Strategy: How I Overcame SMOTE

With unbalanced datasets, a majority of people want to create artificial data through methods such as SMOTE. I believe creating synthetic fraudsters in financial systems ruins the true-world anomaly patterns associated with them. Therefore, I took a penalty approach rather than creating new data.

After completing a stratified split of the original dataframe, I constructed an XGBoost model and used the scale_pos_weight parameter. The algorithm was instructed, "I will not be angry if you block a legitimate customer by mistake but if you miss a fraudster the penalty will be quite large".

The evaluation process, I neglected to use the misleading ROC-AUC curve and concentrated on the Precision-Recall (PR-AUC) curve.

As for the Business ROI and cost simulation, the PR-AUC score from my test dataset was equal to 0.853. This is actually a very positive catch rate for a dataset that haa nimal ratio of imbalance.

However, the technical scores were not the best part; I created a Profit and Loss (Cost Matrix) simulation of the test dataset for business purposes.

The estimated expenses associated with affecting a legitimate customer because of a False Positive are approximately $10 for operational costs (support tickets, phone calls). The estimated losses attributed to missing a fraudster (False Negative) are approximately average cart amounts ($122 approx).

An $11,956 loss would have been incurred by the bank on this testing set only, but because of the deployment of this model they have ultimately gained $9,548 (this amount also considers operational costs related to false alarms). By achieving this level of effectiveness with their customer base during just one year, the bank has the ability to improve their entire budget significantly through a far larger customer base in total over the year being evaluated.

# Limitations and Next Steps: 
Production isn't all sunshine and rainbows. The methods employed by fraudsters change over time (also referred to as Data Drift). Therefore, models cannot simply be built once and left on the shelf - they must be continually retrained with new data.

I've used libraries such as XGBoost and Scikit-Learn heavily to expedite my work. In the next stage, I aim to learn them better and do more competent projects.