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



# Data Engineering

1. In Kinesis Data Stream, number_of_shards = max (incoming_write_bandwidth_in_KB/1000, outgoing_read_bandwidth_in_KB/2000)

   where

   incoming_write_bandwidth_in_KB = average_data_size_in_KB multiplied by the number_of_records_per_seconds. 

   outgoing_read_bandwidth_in_KB = incoming_write_bandwidth_in_KB multiplied by the number_of_consumers.

2. Glue cannot write the output in RecordIO-Protobuf format. Lambda is not suited for long-running processes such as the task of transforming 1TB data into RecordIO-Protobuf format. Kinesis Firehose is not meant to be used for batch processing use cases and it cannot write data in RecorIO-Protobuf format. Apache Spark (running on the EMR cluster in this use-case) can write the output in RecorIO-Protobuf format.

3. Converting the data to recordIO-protobuf file type can significantly improve the training time with a marginal increase in cost to store the recordIO-protobuf data on S3. Spinning up EMR clusters would be costly and require complex infrastructure maintenance.

4. For those SageMaker supervised learning algorithms which require the training data to be in CSV format, the target variable should be in the first column and it should not have a header record. 

5. Kinesis Firehose can transform data to Parquet format and store it on S3 without provisioning any servers. Also this transformed data can be read into an Athena Table via a Glue Crawler and then the underlying data is readily available for ad-hoc analysis. Although Glue ETL Job can transform the source data to Parquet format, it is best suited for batch ETL use cases and it’s not meant to process streaming data. EMR cluster is not an option as the company does not want to manage the underlying infrastructure.

6. Kinesis Data Firehose is used for streaming data scenarios. AWS Glue ML Transforms job can perform deduplication in a serverless fashion.

7. There is no such thing as a LOAD command for Redshift. COPY is much faster than insert. UNLOAD is used to write the results of a Redshift query to one or more text files on Amazon S3.

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

7. 

