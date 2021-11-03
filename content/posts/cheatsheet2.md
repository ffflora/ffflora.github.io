---
title: "AWS Machine Learning Specialty Cheatsheet(2)"
date: 2021-11-03T14:34:57-07:00
draft: false
toc: false
images:
tags:
- AWS
- Machine Learning
categories:	
- Data Science
---



# Exploratory Data Analysis 

1. In case of a binary classification model with strongly unbalanced classes, we need to over-sample from the minority class, collect more training data for the minority class and create more samples using algorithms such as SMOTE which effectively uses a k-nearest neighbours approach to exclude members of the majority class while in a similar way creating synthetic examples of a minority class. Over-sampling from the positive class or collecting more training data for the positive class would further aggravate the situation.

   http://www.svds.com/learning-imbalanced-classes/

   https://stats.stackexchange.com/questions/235808/binary-classification-with-strongly-unbalanced-classes

2. A data **warehouse** can only store **structured** data whereas a data lake can store structured, semi-structured and unstructured data.  

   **<u>Data lakes</u>** provides schema on **read** access, whereas <u>**data warehouse**</u> provides schema on **write**.

3. Great reference for the most common probability distributions: [Common Probability Distributions: The Data Scientist’s Crib Sheet](https://medium.com/@srowen/common-probability-distributions-347e6b945ce4)

   The **Rademacher** distribution takes value 1 with probability 1/2 and value −1 with probability 1/2. The **degenerate** distribution is localized at a point x0, where x is certain to take the value x_0. The probability mass function equals 1 at this point and 0 elsewhere.

4. Tf-idf is a statistical technique frequently used in Machine Learning areas such as text-summarization and classification. Tf-idf measures the **relevance of a word in a document compared to the entire corpus of documents**. You have a corpus (D) containing the following documents:

   Document 1 (d1) : “A quick brown fox jumps over the lazy dog. What a fox!”

   Document 2 (d2) : “A quick brown fox jumps over the lazy fox. What a fox!”

   Which of the following statements is correct:

   - Using tf-idf, the word “fox” is equally relevant for both document d1 and document d2

   **tf** is the frequency of any "term" in a given "document". Using this definition, we can compute the following:

   tf(“fox”, d1) = 2/12 , as the word "fox" appears twice in the first document which has a total of 12 words

   tf(“fox”, d2) = 3/12 , as the word "fox" appears thrice in the second document which has a total of 12 words

   

   An idf is constant per corpus (in this case, the corpus consists of 2 documents) , and accounts for the ratio of documents that include that specific "term". Using this definition, we can compute the following:

   idf(“fox”, D) = log(2/2) = 0 , as the word "fox" appears in both the documents in the corpus

   

   Now,

   tf-idf(“fox”, d1, D) = tf(“fox”, d1) * idf(“fox”, D) = (2/12) * 0 = 0

   tf-idf(“fox”, d2, D) = tf(“fox”, d2) * idf(“fox”, D) = (3/12) * 0 = 0

   Using tf-idf, the word “fox” is equally relevant (or just irrelevant!) for both document d1 and document d2

5. TF-IDF: Three different inverse document frequency functions are **standard, smooth, probabilistic:** Standard: log(N)/n_t, Smooth: log(N)/((1+n_t) +1), Probabilistic: log(N-N_t)/n_t, where N is the total number of documents in the corpus, and n_t is a number of documents where the term t appears.

6. **Logarithm transformation** and **Standardization** are the correct techniques to address outliers in data. Please review this reference link:

   https://towardsdatascience.com/feature-engineering-for-machine-learning-3a5e293a5114

7. ElasticSearch, EMR and EC2 are **not** “serverless”. 

8. The best way to engineer the **cyclical** features is to represent these as (x,y) coordinates on a circle using sin and cos functions. Please review this technique in more detail here -

   http://blog.davidkaleko.com/feature-engineering-cyclical-features.html

9. Q1 = 1/4 x (N+1)th term

   Q3 = 3/4 x (N+1)th term

   Interquartile Range (IQR) = Q3-Q1 

   Minimum outlier cutoff = Q1 - 1.5 * IQR 

   Maximum outlier cutoff = Q3 + 1.5 * IQR 

   More details on the box plot statistical characteristics:

   https://towardsdatascience.com/understanding-boxplots-5e2df7bcbd51

10. The **Box Plot and Violin Plot** are used to summarize multivariate distributions. They are a standardized way of displaying the data distributions based on a **five-number summary (minimum, first quartile (Q1), median, third quartile (Q3), and maximum).** The plots show symmetry, tightness of the groups, skewness, and any outliers present.

11. The Multiple Imputations by Chained Equations (**MICE**) algorithm is a robust, informative method of dealing with **missing data** in your datasets. This procedure imputes or 'fills in' the missing data in a dataset through an iterative series of predictive models. Each specified variable in the dataset is imputed in each iteration using the other variables in the dataset. These iterations will be run continuously until convergence has been met. In General, MICE is a better imputation method than naive approaches (filling missing values with 0, dropping columns).

12. QuickSight supports UTF-8 file encoding, **but not UTF-8 (with BOM)**.

13. Quicksight supports the following file formats only: **CSV/TSV, ELF/CLF, JSON, XLSX**.

14. For QuickSight, AWS Enterprise Edition Authors (create & publish) pay $18/month with an annual subscription, while Readers (getting secure access to interactive dashboards) pay $0.30/session up to $5/month'.

15. **F0.5-Measure** (beta=0.5): More weight on precision, less weight on recall.

     **F1-Measure** (beta=1.0): Balance the weight on precision and recall.

     **F2-Measure** (beta=2.0): Less weight on precision, more weight on recall

    ![img](https://img-c.udemycdn.com/redactor/raw/test_question_description/2020-12-06_07-55-05-6516021657c5e962ac6ca87cdd939d92.jpg)

16. The <u>*Recall*</u> is also called **Sensitivity, Hit Rate, and True Positive Rate.**

    **Positive Predictive Value (PPV)** is the same as <u>*Precision*</u>.

17. The Receiver Operating Characteristic - **ROC**, **true positive rate & false positive rate** - determines the ability of a binary classification model, as its discrimination threshold is varied.

18. **False positive Rate** = 1 - TNR(True negative rate) = 1 - specifiticy 

19. **Specificity** = TN/(TN+FP)

20. If the model has a high specificity, it implies that all false positives (think of it as false alarms) have been weeded out. In other words, the **specificity** of a test refers to how well the test identifies those who have not indulged in substance abuse.

     Please read this excellent reference article for more details:

     https://www.statisticshowto.datasciencecentral.com/sensitivity-vs-specificity-statistics/

21. As per sklearn, **the minority class is considered as the positive class.** Hence, in cases with **fraudulent** data, a fraud transaction is considered as a positve class. Similary, in diagnostics, a disease detected is considered positive.

22. **Type 2** error is also known as **False Negative.**

    A Null hypothesis assumes positive for no-change/default (No Fraud, Healthy, Not Guilty), and a negative for change/non-default (Not Healthy, Fraud, Guilty) outcome.

    A type 2 error occurs when the null hypothesis is false but is falsely accepted. This corresponds to the False-negative in classification, where a negative is considered for no-change/default (No Fraud, Healthy, Non-Guilty), etc.

    A type 1 error occurs when the null hypothesis is true but is falsely rejected.

23. **Miss Rate is also known as the False Negative Rate**. It is given as FN/FN+TP (FN =False Negative, TP = True Positive).

     As the False Negatives are undesired and should be reduced to zero for an ideal model, the value of the miss rate in the ideal case will approach zero.

24. If there are <u>no outliers,</u> **MAE** (Mean Absolute Error) will be more suitable  for comparison of performances of various models, as the error remains linear in this case. And if there <u>are outliers</u> **RMSE** will be preferred.

25. **Ground truth** provides built-in five data labelling tasks

        ```
        Bounding Boxes
        Image classification
        Semantic segmentation
        Text classification
        Named Entity Recognition.
        ```

26. The *p* value represents the level of probability that an apparently significant relationship between variables was really just due to chance. <u>If *p* is set at 0.01, this means that we would expect such a result in only 1 in 100 cases</u>. This is a very stringent level, and while it means that the researcher can be more confident about a significant result if they find one, **it also increases the chance of making a Type II error: confirming the null hypothesis when it should be rejected.**

27. Adoptive (or q-quantile) binning helps in partitioning a numeric attribute into 'q' equal partitions.  Adoptive binning leads to discrete-valued categorical features transforming **numerical data into ordinal data.** 

28. 95% of the measurements fall between +/- 2 standard deviations around the mean. 

     

## Model Performance Evaluation

### Regression Model

1. MSE
2. RMSE

**Residual** is Actual - Predicted.

**MSE** is the mean value of (sumation of each residual^2)

**RMSE** is rooted MSE

### Binary Model and Multiclass Classifier 

<q><i>The actual output of many binary classification algorithms is a prediction score. The score indicates the system’s certainty that the given observation belongs to the positive class</i></q>

To convert this raw score to a positive or negative class, we need to specify a **cut-off**. A sample with score greater than the cut-off is classified as positive class and a sample with score less than the cut-off is classified as negative class.

 Instead of manually performing this step, we can compute "AUC" metric.  AUC refers to Area Under Curve.  **The curve here refers to the plot that has Probability of False Alarm (False Positive Rate) in X-Axis and Probability of Detection (Recall) in Y-Axis.** By plotting False Alarm vs Recall at different cut-off thresholds, we can form a curve.  It measures the ability of the model to predict a higher score for positive examples as compared to negative examples. Since AUC is independent of the selected threshold, you can get a sense of the prediction performance of your model from the AUC metric without picking a threshold.

Common Techniques for evaluating performance:

1. Visually observe raw score using Plots
2. Evaluate Area Under Curve (AUC) Metric

3. Confusion Matrix



## Some Terms

**Early stopping**: the model trains until it stops improving. Early stopping is a **form of regularization used to avoid overfitting when training a learner with an** iterative method

**Bias:** does not match the reality.

**High bias:** The model doesn't learn from data, and it translate to large training and validation errors. In other words, the model is **underfitting.** Should **decrease** regularizations.

**Low bias:** Overfitting. 

**Variance:** Measures how well the algorithm generalizes from the data, it's the difference between the validation data and training data. 

**High Variance:** Validation error is high but training error is low: overfitting. Should **increase** regularizations.

**Regularization**: tone down the overdependence of specific features.

**L1 Regularization: ** Algorithm aggresively eliminates features that are not important. Useful in large demension dataset - reduce the number of features. L1 gives you sparse estimates.

**L2 Regularization:** Algorithm simply reuces the weight of features. It allows other features to influence outcome. L2 gives you dense estimates. 

**Inference**: The process of using the trained model to make **predictions**.

**Normalization:** The normalization transformer normalizes numeric variables to have a mean of zero and variance of one.
