---
title: "AWS Machine Learning Specialty Knowledge Pickup"
date: 2021-04-13T23:26:49-07:00
draft: false
toc: false
images:
tags:
- AWS
- Machine Learning
categories:	
- Data Science
---



This notes is some catchups for better prep for AWS machine learning specialty cert.

# Data Engineering

1. In Kinesis Data Stream, number_of_shards = max (incoming_write_bandwidth_in_KB/1000, outgoing_read_bandwidth_in_KB/2000)

   where

   incoming_write_bandwidth_in_KB = average_data_size_in_KB multiplied by the number_of_records_per_seconds. 

   outgoing_read_bandwidth_in_KB = incoming_write_bandwidth_in_KB multiplied by the number_of_consumers.

2. **Glue cannot write the output in RecordIO-Protobuf forma**t. Lambda is not suited for long-running processes such as the task of transforming 1TB data into RecordIO-Protobuf format. Kinesis Firehose is not meant to be used for batch processing use cases and it cannot write data in RecorIO-Protobuf format. Apache Spark (running on the EMR cluster in this use-case) can write the output in RecorIO-Protobuf format.

3. Converting the data to recordIO-protobuf file type can significantly improve the training time with a marginal increase in cost to store the recordIO-protobuf data on S3. Spinning up EMR clusters would be costly and require complex infrastructure maintenance.

4. For those SageMaker supervised learning algorithms which require the training data to be in CSV format, the target variable should be in the first column and it should not have a header record. 

5. Kinesis Firehose can transform data to Parquet format and store it on S3 without provisioning any servers. Also this transformed data can be read into an Athena Table via a Glue Crawler and then the underlying data is readily available for ad-hoc analysis. Although Glue ETL Job can transform the source data to Parquet format, it is best suited for batch ETL use cases and it’s not meant to process streaming data. EMR cluster is not an option as the company does not want to manage the underlying infrastructure.

6. **Kinesis Data Firehose is used for streaming data scenarios**. **AWS Glue ML Transforms job can perform deduplication** in a serverless fashion.

7. There is no such thing as a LOAD command for Redshift. COPY is much faster than insert. UNLOAD is used to write the results of a Redshift query to one or more text files on Amazon S3.

8. If you want Amazon SageMaker to replicate a subset of data on each ML compute instance that is launched for model training, specify `ShardedByS3Key` for S3DataDistributionType field.

9. Kinesis Product Library provides built-in performance benefits and ease of use advantages. Please review more details here:

   https://docs.aws.amazon.com/streams/latest/dev/developing-producers-with-kpl.html#developing-producers-with-kpl-advantage

10. Kinesis Data Streams PutRecord API uses name of the stream, a partition key and the data blob whereas Kinesis Data Firehose PutRecord API uses the name of the delivery stream and the data record.

11. Hyperparameters should be tuned against the Validation Set.



# Exploratory Data Analysis 

1. In case of a binary classification model with strongly unbalanced classes, we need to over-sample from the minority class, collect more training data for the minority class and create more samples using algorithms such as SMOTE which effectively uses a k-nearest neighbours approach to exclude members of the majority class while in a similar way creating synthetic examples of a minority class. Over-sampling from the positive class or collecting more training data for the positive class would further aggravate the situation.

    http://www.svds.com/learning-imbalanced-classes/

   https://stats.stackexchange.com/questions/235808/binary-classification-with-strongly-unbalanced-classes

2. A data warehouse can only store structured data whereas a data lake can store structured, semi-structured and unstructured data.  

3. Great reference for the most common probability distributions: [Common Probability Distributions: The Data Scientist’s Crib Sheet](https://medium.com/@srowen/common-probability-distributions-347e6b945ce4)

4. You can use Inference Pipeline to package Spark and scikit-learn based preprocessors into containers:

   https://docs.aws.amazon.com/sagemaker/latest/dg/inference-pipeline-mleap-scikit-learn-containers.html

5. Tf-idf is a statistical technique frequently used in Machine Learning areas such as text-summarization and classification. Tf-idf measures the relevance of a word in a document compared to the entire corpus of documents. You have a corpus (D) containing the following documents:

   Document 1 (d1) : “A quick brown fox jumps over the lazy dog. What a fox!”

   Document 2 (d2) : “A quick brown fox jumps over the lazy fox. What a fox!”

   Which of the following statements is correct:

   - Using tf-idf, the word “fox” is equally relevant for both document d1 and document d2

   

   tf is the frequency of any "term" in a given "document". Using this definition, we can compute the following:

   tf(“fox”, d1) = 2/12 , as the word "fox" appears twice in the first document which has a total of 12 words

   tf(“fox”, d2) = 3/12 , as the word "fox" appears thrice in the second document which has a total of 12 words

   

   An idf is constant per corpus (in this case, the corpus consists of 2 documents) , and accounts for the ratio of documents that include that specific "term". Using this definition, we can compute the following:

   idf(“fox”, D) = log(2/2) = 0 , as the word "fox" appears in both the documents in the corpus

   

   Now,

   tf-idf(“fox”, d1, D) = tf(“fox”, d1) * idf(“fox”, D) = (2/12) * 0 = 0

   tf-idf(“fox”, d2, D) = tf(“fox”, d2) * idf(“fox”, D) = (3/12) * 0 = 0

   Using tf-idf, the word “fox” is equally relevant (or just irrelevant!) for both document d1 and document d2

   

   You can further read about tf-idf on this reference link:

   https://en.wikipedia.org/wiki/Tf–idf

6. Logarithm transformation and Standardization are the correct techniques to address outliers in data. Please review this reference link:

   https://towardsdatascience.com/feature-engineering-for-machine-learning-3a5e293a5114
   
7. ElasticSearch, EMR and EC2 are not “serverless”. 

8. The best way to engineer the cyclical features is to represent these as (x,y) coordinates on a circle using sin and cos functions. Please review this technique in more detail here -

    http://blog.davidkaleko.com/feature-engineering-cyclical-features.html

9. Interquartile Range (IQR) = Q3-Q1 

    Minimum outlier cutoff = Q1 - 1.5 * IQR 

    Maximum outlier cutoff = Q3 + 1.5 * IQR 

    More details on the box plot statistical characteristics:

    https://towardsdatascience.com/understanding-boxplots-5e2df7bcbd51

    


# Modeling

1. Object2Vec can be used to find semantically similar objects such as questions. BlazingText Word2Vec can only find semantically similar words. 

2. **Incremental Training** in Amazon SageMaker

   Over time, you might find that a model generates inference that are not as good as they were in the past. With incremental training, you can use the artifacts from an existing model and use an expanded dataset to train a new model. Incremental training saves both time and resources.

   Use incremental training to:

   - Train a new model using an expanded dataset that contains an underlying pattern that was not accounted for in the previous training and which resulted in poor model performance.
   - Use the model artifacts or a portion of the model artifacts from a popular publicly available model in a training job. You don't need to train a new model from scratch.
   - Resume a training job that was stopped.
   - Train several variants of a model, either with different hyperparameter settings or using different datasets.

3. Blazing Text algorithm can be used in both supervised and unsupervised learning modes.

4. SageMaker DeepAR algorithm specializes in forecasting new product performance. 

5. Specificity = (True Negatives / (True Negatives + False Positives))

   If the model has a high specificity, it implies that all false positives (think of it as false alarms) have been weeded out. In other words, the **specificity** of a test refers to how well the test identifies those who have not indulged in substance abuse.

   Please read this excellent reference article for more details:

   https://www.statisticshowto.datasciencecentral.com/sensitivity-vs-specificity-statistics/

6. LDA: Observations are referred to as documents. The feature set is referred to as vocabulary. A feature is referred to as a word. And the resulting categories are referred to as topics.

7. **mode** is the mandatory hyperparameter for both the Word2Vec (unsupervised) and Text Classification (supervised) modes of the SageMaker BlazingText algorithm.

8. Factorization Machines algorithm specializes in building recommendation systems.

9. Image Classification is used to classify images into multiple classes such as cat vs dog. Object Detection is used to detect objects in an image. Semantic Segmentation is used for pixel level analysis of an image and it can be used in this computer vision system to detect misalignment

10. feature_dim and k are the required hyperparameters for the SageMaker K-means algorithm

11. When you use automatic model tuning, the linear learner internal tuning mechanism is turned off automatically. This sets the number of parallel models, num_models, to 1.

12. AUC/ROC  is the best evaluation metric for a binary classification model. this metric does not require you to set a classification threshold.

    For imbalanced datasets, you are better off using another metric called - **PR AUC** - that is also used in production systems for a highly imbalanced dataset, where the fraction of positive class is small, such as in case of credit card fraud detection.

13. You can think of L1 as reducing the number of features in the model altogether. L2 “regulates” the feature weight instead of just dropping them. Please review the concept of L1 and L2 regularization in more detail:

    https://towardsdatascience.com/l1-and-l2-regularization-methods-ce25e7fc831c

14. Factorization Machine can be used to capture click patterns for a click prediction system:

15. The residuals plot would indicate any trend of underestimation or overestimation. Both Mean Absolute Error and RMSE would only give the error magnitude. AUC is a metric used for classification models.

16. LDA is a "bag-of-words" model, which means that the order of words does not matter

    

#  ML Implementation and Operation

1. Inference Pipeline can be considered as an Amazon SageMaker model that you can use to make either real-time predictions or to process batch transforms directly without any external preprocessing.

2. Neo currently supports image classification models exported as frozen graphs from TensorFlow, MXNet, or PyTorch, and XGBoost models.:

   https://docs.aws.amazon.com/sagemaker/latest/dg/neo.html

3. The default Sagemaker IAM role gets permissions to access any bucket that has sagemaker in the name. If you add a policy to the role that grants the SageMaker service principal S3FullAccess permission, the name of the bucket does not need to contain sagemaker. Granting public access to S3 bucket is not recommended. You can read further on this -

   https://docs.aws.amazon.com/sagemaker/latest/dg/gs-config-permissions.html

4. version tag should be used in the Registry Paths:

   “:1” is the correct version tag for **production** systems.

5. SageMaker does not support resource based policies. You can create a role to delegate access or provide access via identity federation. 

6. If you want Amazon SageMaker to replicate a subset of data on each ML compute instance that is launched for model training, specify `ShardedByS3Key` for S3DataDistributionType field.

7. There are no inter-node communications for batch processing, so inter-node traffic encryption is not required. SSH and AWS-SSE are not used for inter-node traffic encryption.

8. If the value of the objective metric for the current training job is worse (higher when minimizing or lower when maximizing the objective metric) than the median value of running averages of the objective metric for previous training jobs up to the same epoch, Amazon SageMaker stops the current training job.

9. Within an inference pipeline model, Amazon SageMaker handles invocations as a sequence of HTTP requests.

10. Only three built-in algorithms currently support incremental training: Object Detection Algorithm, Image Classification Algorithm, and Semantic Segmentation Algorithm:

11. By using Amazon Elastic Inference (EI), you can speed up the throughput and decrease the latency of getting real-time inferences from your deep learning models that are deployed as Amazon SageMaker hosted models, but at a fraction of the cost of using a GPU instance for your endpoint.

12. Network isolation is not supported by the following managed Amazon SageMaker containers as they require access to Amazon S3:

    Chainer

    PyTorch

    Scikit-learn

    Amazon SageMaker Reinforcement Learning

    

---

# Summary

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





## SageMaker Service Overview

### Data Copy from S3 to Training Instance 

**File Mode:**

- Training job copies entire dataset from S3 to training instance
- Space Needed: Entire data set + Final model artifacts

**Pipe Mode**: 

- Training job streams data from S3 to training instance 
- Faster start time and Better Throughput 
- Space Needed: Final model artifacts

### Data Format in SageMaker

#### Training Data Format

CSV

RecordIO: Data types needs to be int32, float 32, float 64

Algorithm Specipic formats( LibSVM, JSON, Parquet)

Data needs to be stored in S3

#### Infetence Format

CSV

JSON

RecordIO

## SageMaker Build-in Algos

**BlazingText**

Unsupervised -> word2vec

Supervised -> multiclass, multilebel classification

{{< image src="/1.png"  position="center" style="border-radius: 8px;" >}}

**Object2Vec**

Supervised -> Classification, Regression 

**Factorization Machines**

Supervised -> Classification, Regression

**K-Nearest Neighbors**

Supervised -> Classification, Regression

**Linear Learner**

Supervised -> Classification, Regression

**XGBoost**

Supervised -> Classification, Regression

**DeepAR**

Supervised -> Timeseries Forecasting 

**Object Detection**

Supervised -> Classification 

**Image Classification**

Supervised -> Classification 

**Semantic Segmentation**

Supervised -> Classification 

**Seq2Seq**

Supervised -> Convert seq of tokens to another seq to tokens

**K-Means**

Unsupervised -> clsutering 

**LDA**

Unsupervised -> Topic Moeling (Document level)

**Neural Topic Model(NTM)**

Unsupervised -> Topic modeling, similiar to LDA

**PCA: Principal Component Analysis**

Unsupervised -> Dimensioality reduction 

**Random Cut Forest**

Unsupervised -> anomaly detection 

**IP Insights**

Unsupervised -> Detect unusual network activity 





 











