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



This notes contains some catchups for better preping for AWS machine learning specialty cert.

# Data Engineering

1. In <u>Kinesis Data Stream</u>, number_of_shards = max (incoming_write_bandwidth_in_KB/**1000**, outgoing_read_bandwidth_in_KB/**2000**)

   where

   incoming_write_bandwidth_in_KB = average_data_size_in_KB multiplied by the number_of_records_per_seconds. 

   outgoing_read_bandwidth_in_KB = incoming_write_bandwidth_in_KB multiplied by the number_of_consumers.

2. **KCL** helps you consume and process data from a **Kinesis data stream** by taking care of many of the complex tasks associated with distributed computing. These include load balancing across multiple consumer application instances, responding to consumer application instance failures, checkpointing processed records, and reacting to resharding. The KCL takes care of all of these subtasks so that you can focus your efforts on **writing your custom** record-processing logic.

3. **Glue <u>cannot</u> write the output in RecordIO-Protobuf forma**t. Lambda is not suited for long-running processes such as the task of transforming 1TB data into RecordIO-Protobuf format. Kinesis **Firehose** is not meant to be used for batch processing use cases and **it cannot write data in RecorIO-Protobuf format**. Apache Spark (running on the EMR cluster in this use-case) can write the output in RecorIO-Protobuf format.

4. Glue's **FindMatches** feature is a new way to perform de-duplication as part of Glue ETL, and is a simple, server-less solution to the problem.

5. AWS Glue does **not** support ***timestream*** as the source input type.

6. Converting the data to **recordIO-protobuf** file type can significantly improve the training time with a marginal increase in cost to store the recordIO-protobuf data on **S3**. Spinning up EMR clusters would be costly and require complex infrastructure maintenance.

7. For those SageMaker supervised learning algorithms which require the training data to be in **CSV** format, the target variable should be in the **first column** and it **should not have a header** record. 

8. Kinesis Firehose can transform data to **Parquet** format and store it on S3 without provisioning any servers. Also this transformed data can be read into an Athena Table <u>via a Glue Crawler</u> and then the underlying data is readily available for ad-hoc analysis. Although Glue ETL Job can transform the source data to Parquet format, it is best suited for batch ETL use cases and it’s not meant to process streaming data. EMR cluster is not an option as the company does not want to manage the underlying infrastructure.

9. **Athena** performs much more efficiently and at lower cost when using columnar formats such as **Parquet or ORC**, and that Kinesis Firehose has the ability to convert **JSON** <u>data to Parquet or ORC format on the fly.</u>

10. **Athena** supports a wide variety of data formats such as **CSV, JSON, ORC, Avro, or Parquet.**

11. **Athena** automatically executes queries in **parallel**, so that you get query results in seconds, even on large dataset;  Athena leverages ***Hive*** for **partitioning** data. There are two types of **cost controls** in Amazon Athena:

    – **per-query limit**

    **– per-workgroup limit**

    If the query in Athena **exceeded the limit**, all the succeeding queries will be canceled. Therefore, the type of cost control you need to use is the **per-query limit or the per query data usage control**. The per-query control limit **specifies the total amount of data scanned per query.** You can create only one per-query control limit in a workgroup, and it applies to each query that runs in it.

12. **Athena** integrates with Amazon **QuickSight** for easy data visualization.

13. Kinesis Data Firehose needs the following three elements to convert the input data format to Parquet/ORC format:

    1) **Deserializer to read JSON data** (Input must be in JSON only, If not use a lambda to convert to JSON),

    2) Schema using **AWS Glue to interpret the input data,**

    3) Serializer to convert the data to target columnar storage format (Parquet/ORC).

14. **A Glue crawler connects to the data store that can be Amazon S3, RDS, Redshift, DynamoDB, or JDBC (Java Database Connectivity Interface).**

    Amazon ElastiCache is in-memory data stores in the cloud that cannot be connected to Glue. (A caveat: a user can write custom Scala or Python code and import custom libraries and Jar files into Glue ETL jobs to access data sources not natively supported by AWS Glue, like ElastiCache).

15. **Kinesis Data Firehose is used for streaming data scenarios**. **AWS Glue ML Transforms job can perform deduplication** in a serverless fashion.

16. **Amazon Kinesis Data Firehose** **<u>*buffers incoming streaming*</u>** data to a certain size or for a certain period of time before delivering it to destinations. You can configure the buffer size and buffer interval while <u>creating your **delivery stream**.</u>

    Buffer size is in MBs and ranges from <u>1MB to 128MB</u> for Amazon S3 destination and <u>**1MB to 100MB**</u> for Amazon Elasticsearch Service destination. Buffer interval is in seconds and ranges from **60 seconds to 900 seconds**. Please note that in circumstances where data delivery to destination is falling behind data writing to a delivery stream, Firehose raises buffer size dynamically to catch up and make sure that all data is delivered to the destination.

    ![img](https://img-c.udemycdn.com/redactor/raw/test_question_description/2020-12-04_16-41-13-3dcfe5855fa5c240d34bedca038dcb9b.JPG)

17. Kinesis Firehose Data **Delivery Failure Handing** 

    ![](https://img-c.udemycdn.com/redactor/raw/test_question_description/2020-12-05_02-32-25-005129db5966b762cc65018c53796c0e.JPG)

18. Firehose can deliver data to **S3, Elastic Search, Splunk, Redshift, and HTTP Endpoints.** Athena is not a database, and Firehose **doesn't** deliver data to DynamoDB.

19. There is no such thing as a ~~LOAD~~ command for **Redshift**. ***COPY*** is much faster than ***INSERT***. *UNLOAD* is used to write the results of a Redshift query to one or more text files on Amazon S3.

20. Only **GZIP** is supported if the data is loaded to Amazon **Redshift**.

21. **Workload management (WLM)** is only available in Amazon **Redshift**.

22. If you want Amazon SageMaker to replicate a subset of data on each ML compute instance that is launched for model training, specify ***`ShardedByS3Key`*** for <u>**S3DataDistributionType**</u> field.

23. Amazon **KPL** is only used for <u>streaming data into Amazon Kinesis Data Streams</u> and not for reading data from it.

24. **Kinesis Product Library**(KPL) provides built-in performance benefits and ease of use advantages. Please review more details here:

    https://docs.aws.amazon.com/streams/latest/dev/developing-producers-with-kpl.html#developing-producers-with-kpl-advantage

25. Hyperparameters should be tuned against the **Validation** Set.

26. Kinesis cheatsheet https://tutorialsdojo.com/amazon-kinesis/

27. Amazon **FSx** for **Lustre** <u>speeds up your **File mode** training jobs by serving your Amazon S3 data to Amazon SageMaker at high speeds</u>. The first time you run a training job, Amazon FSx for Lustre automatically copies data from Amazon S3 and makes it available to Amazon SageMaker. Additionally, the same Amazon FSx file system can be used for subsequent iterations of training jobs on Amazon SageMaker, preventing repeated downloads of common Amazon S3 objects. Because of this, Amazon FSx has the most benefit to training jobs that **have training sets in Amazon S3 and in workflows where training jobs must be run several times using different training algorithms or parameters to see which gives the best result.**

28. Data storage for machine learning modeling could be from **S3**, **Amazon Fsx for Lustre, and Amazon EFS.**

29. **AWS Database Migration Service (AWS DMS)** is a cloud service that makes it easy to migrate relational databases, data warehouses, NoSQL databases, and other types of data stores. You can use AWS DMS to migrate your data into the AWS Cloud, between on-premises instances (through an AWS Cloud setup), or between combinations of cloud and on-premises setups. With AWS DMS, you can perform one-time migrations, and you can replicate ongoing changes to keep sources and targets in sync.

    - continuous Data Replication
    - No data transformation (**ETL jobs needs to be done later somewhere else**.)
    - once the data is in AWS, you can use <u>Glue</u> to transform it. 
    - You can use Database Migration Service for one-time data migration into RDS and EC2-based databases.
    - You can also continuously replicate your data with high availability (enable multi-AZ) and consolidate databases into a petabyte-scale data warehouse by streaming data to Amazon Redshift and Amazon S3.
    - Replication between on-premises to on-premises databases is not supported.
    - The service provides an end-to-end view of the data replication process, including diagnostic and performance data for each point in the replication pipeline.
    - You only pay for the compute resources used during the migration process and any additional log storage. Each database migration instance includes storage sufficient for swap space, replication logs, and data cache for most replications and inbound data transfer is free.

30. The **S3 Standard-IA** storage class is designed for long-lived and infrequently accessed data. (IA stands for *infrequent access*.) S3 Standard-IA is available for **millisecond access** (same as the S3 Standard storage class). Amazon S3 charges a retrieval fee for these objects, so they are most suitable for infrequently accessed data.

    Amazon S3 **Glacier Deep Archive** does not provide immediate access. Expedited retrievals may still take **hours** to complete.

31. **S3** related: 

    - By default, you can create up to **100** buckets in **each** of your AWS accounts.
    - You **can’t change its Region** after creation.
    - You can host **static** websites by configuring your bucket for website hosting.
    - You **can’t delete** an S3 bucket using the Amazon S3 console if the bucket contains **100,000** or more objects. You **can’t** delete an S3 bucket using the **AWS CLI if versioning is enabled**.

32. **S3 Data Consistency Model**

    - read-after-write consistency for PUTS of new objects in your S3 bucket in all regions
    - strong consistency for read-after-write HEAD or GET requests
    - strong consistency for overwrite PUTS and DELETES in all regions
    - strong read-after-write consistency for any storage request
    - eventual consistency for listing all buckets after deleting a bucket (deleted bucket might still show up)
    - eventual consistency on propagation of enabling versioning on a bucket for the first time.

33. **S3 Select**

    - S3 Select is an Amazon S3 capability designed to **pull out only the data you need** from an object, which can dramatically improve the performance and reduce the cost of applications that need to access data in S3.
    - Amazon S3 Select works on objects stored in **CSV and JSON format, Apache Parquet format, JSON Arrays, and BZIP2 compression for CSV and JSON objects.**
    - **CloudWatch** Metrics for S3 Select lets you monitor S3 Select usage for your applications. These metrics are available at **1-minute** intervals and lets you quickly identify and act on operational issues.

34. **Encryption** in S3:

    - - **Server-side** Encryption using
        - **Amazon S3-Managed Keys (SSE-S3)**
        - **AWS KMS-Managed Keys (SSE-KMS)**
        - **Customer-Provided Keys (SSE-C)**
      - **Client-side** Encryption using
        - AWS KMS-managed customer master key
        - client-side master key

35. **Availability Zones** give customers the ability to operate production applications and databases that are more highly available, fault-tolerant, and scalable than would be possible from a single data center. AWS maintains multiple AZs around the world and more zones are added at a fast pace. Each AZ can be multiple data centers (typically 3), and at full scale can be hundreds of thousands of servers. They are fully isolated partitions of the AWS Global Infrastructure. With their own power infrastructure, the AZs are physically separated by a meaningful distance, many kilometers, from any other AZ, although all are within 100 km (60 miles of each other).

    All AZs are interconnected with high-bandwidth, low-latency networking, over fully redundant, dedicated metro fiber providing high-throughput, low-latency networking between AZs. The network performance is sufficient to accomplish synchronous replication between AZs. AWS Availability Zones are also powerful tools for helping build highly available applications. AZs make partitioning applications about as easy as it can be. If an application is partitioned across AZs, companies are better isolated and protected from issues such as lightning strikes, tornadoes, earthquakes and more.

36. **Amazon Athena** is an interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL. Athena is **serverless**, so there is no infrastructure to manage, and you pay only for the queries that you run.

    Athena can run queries to analyze unstructured, semi-structured, and structured data stored in S3.

37. **Kinesis** and **Athena** can feed data into **MapReduce** jobs.

38. In Kinesis Data Stream, the minimum value of a stream’s time period is 24 hours, but this can be increased up to 8760 hours (365 days).

39. **AWS Lake formation** integrates with the following services which also accepts the lake formation permissions:

    1) AWS Glue,

    2) Amazon Athena,

    3) Amazon Redshift Spectrum,

    4) Amazon Quicksight,

    5) Amazon EMR and

    6) AWS Glue DataBrew. 

40. **An EMR cluster has three nodes:**

    1) **Master** node - Runs and manages the master components of distributed applications.

    2) **Core** node - Coordinates the data storage as part of the HDFS file system with the task nodes.

    3) **Task** nodes - Optional nodes, and can be added to perform parallel computation tasks on data.

    Task nodes can be used with <u>spot EC2 instances, as they can be used and removed whenever required, and this does not affect the working of other nodes.</u>

    Using spot instances can provide a huge cost saving compared to the on-demand and reserved instances, as this spares the EC2 instances once the job is done. The master and core nodes can't be used with the spot instances, as their availability is a must requirement for the EMR cluster.

41. While you can use Spot instances on any node type, a Spot interruption on the Master node requires terminating the entire cluster, and on a Core node, it can lead to HDFS data loss.

42. **EMR Storage**

    - HDFS
    - EMRFS: access to S3 as if it were HDFS
    - Local file system 
    - EBS for HDFS

43. **<u>Kinesis Data Streams PutRecord API</u>** uses ***name of the stream***, ***a partition key*** and the ***data blob*** whereas <u>**Kinesis Data Firehose PutRecord API**</u> uses the ***name of the delivery stream*** and the ***data record***.

44. ![img](https://img-c.udemycdn.com/redactor/raw/test_question_description/2020-12-06_05-31-48-e1d0b84a350d082ddd023606a22f2432.jpg)

    Kinesis Data Firehose requires only Stream Data Name and the Data Record, whereas the Kinesis Data Stream needs ShardId, Partition Key, and the Data Record.

    Also, Kinesis Firehose scales automatically depending on the input data stream, without any need to manually increase the ingestion size.

45. Using SQS would require implementing substantial functionality on top of SQS, and AWS **Batch is only designed for scheduling and allocating the resources needed for batch processing.**

46. **AWS Data Pipeline** provides a managed orchestration service that gives you greater flexibility in terms of the execution environment, access and control over the compute resources that run your code, as well as the code itself that does data processing. AWS Data Pipeline **launches compute resources** in your account **allowing you direct access to the Amazon EC2 instances or Amazon EMR clusters.**

47. **Preventative** controls encompass the AWS **CAF**(Cloud Adoption Framework) Security Perspective capabilities of IAM, infrastructure security, and data protection. They are **(i) IAM, (ii) Infrastructure security, and (iii) Data protection (encryption and tokenization).**

    **Detective** controls detect changes to configurations or undesirable actions and enable timely incident response. They are **(i) Detection of unauthorized traffic, (ii) Configuration drift, (iii) Fine-grained audits.**

48. AWS Step Functions lets you coordinate multiple AWS services into serverless workflows so you can build and update apps quickly. Using Step Functions, you can design and run workflows that stitch together services, such as AWS Lambda, AWS Fargate, and Amazon SageMaker, into feature-rich applications. Workflows are made up of a series of steps, **with the output of one step acting as input into the next**. They are perfect to run multiple ETL jobs that hand off the input to ML workflows. **(The keyword in the question is 'workflow'.)**

49. **Kinesis Data Analytics** can use **lambda to convert GZIP** and can run SQL on the converted data. A specialist can simply select an AWS Lambda function from the Kinesis Analytics application source page in the AWS Management console. Kinesis Analytics application will automatically process raw data records using the Lambda function and send transformed data to SQL code for further processing.

50. **Object tags** are useful for **security and lifecycle management.** One can use tags to identify an object with, say PII (Personally identifiable information), or use S3 for one-dimensional object categorization. One can also add another dimension using bucket prefixes.

51. **Collaborative filtering** is based on (user, item, rating) tuples. So, unlike content-based filtering, it leverages other users’ experiences. The main concept behind collaborative filtering is that users with similar tastes (based on observed user-item interactions) are more likely to have similar interactions with items they haven't seen before.

    Compared to content-based filtering, collaborative filtering provides better results for **diversity** (how dissimilar recommended items are); **serendipity** (a measure of how surprising the successful or relevant recommendations are); and **novelty** (how unknown recommended items are to a user).

52. Amazon EMR offers you two options to work with Jupyter notebooks: (i) EMR Notebook, and (ii) JupyterHub. An EMR notebook is a 'serverless' Jupyter notebook. Unlike a traditional notebook, the contents of an EMR notebook itself — the equations, visualizations, queries, models, code, and narrative text — are saved in Amazon S3 separately from the cluster that runs the code.

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

     

# Modeling

1. ***Object2Vec*** can be used to find semantically similar **objects** such as questions. ***BlazingText Word2Vec*** can only find semantically similar **words**. 

2. **mode** is the mandatory hyperparameter for both the **Word2Vec** (unsupervised) and **Text Classification** (supervised) modes of the SageMaker BlazingText algorithm.

3. **Incremental Training** in Amazon SageMaker

   Over time, you might find that a model generates inference that are not as good as they were in the past. With incremental training, you can use the artifacts from **an existing model and use an expanded dataset** to train a new model. Incremental training **saves both time and resources**.

   Use incremental training to:

   - Train a new model using an expanded dataset that contains an underlying pattern that was not accounted for in the previous training and which resulted in poor model performance.
   - Use the model artifacts or a portion of the model artifacts from a popular publicly available model in a training job. You don't need to train a new model from scratch.
   - Resume a training job that was stopped.
   - Train several variants of a model, either with different hyperparameter settings or using different datasets.

4. **Rekognition** **doesn't** support **Incremental training**.

5. **Amazon Rekognition Content Moderation** enables you to streamline or automate your image and video **moderation** workflows using machine learning. Using fully managed image and video moderation APIs, you can proactively detect inappropriate, unwanted, or offensive content containing nudity, suggestiveness, violence, and other such categories.

6. Only **three** built-in algorithms currently ***support*** **incremental training**: Object **Detection Algorithm, Image Classification Algorithm, and Semantic Segmentation Algorithm.**

7. **BlazingText** algorithm can be used in both **supervised and unsupervised** learning modes.

8. SageMaker **DeepAR** algorithm specializes in **forecasting** new product performance. 

9. **LDA**: **Observations** are referred to as documents. The **feature set** is referred to as **vocabulary**. A feature is referred to as a **word**. And the **resulting categories** are referred to as topics.

10. **LDA** is a **"bag-of-words"** model, which means that the **order** of words does **not matter**.

11. **Factorization Machines** algorithm specializes in building **recommendation systems.**

12. **Factorization Machine** can be used to **capture click patterns** for a click prediction system.

13. **Image Classification** is used to **classify** images into multiple classes such as cat vs dog. **Object Detection** is used to **detect** objects in an image. Semantic Segmentation is used for pixel level analysis of an image and it can be used in this computer vision system to **detect misalignment**.

14. ***Object Detection*** : is the technology that is related to computer vision and image processing. It's aim? detect objects in an image.

    ***Semantic Segmentation*** : is a technique that detects , for each pixel , the object category it belongs to , all object categories ( labels ) must be known to the model.

    ***Instance Segmentation*** : same as Semantic Segmentation, but dives a bit deeper, it identifies , for each pixel, the object instance it belongs to. The main difference is that differentiates two objects with the same labels in comparison to semantic segmentation.

15. `feature_dim` and `k` are the required hyperparameters for the SageMaker **K-means** algorithm.

16. When you use **automatic model tuning**, the **linear learner** internal tuning mechanism is turned **off** automatically. **This sets the number of parallel models, num_models, to 1.**

17. You can think of **L1** as reducing the number of features in the model altogether. **L2** **“regulates” the feature weight** instead of just dropping them. Please review the concept of L1 and L2 regularization in more detail:

    https://towardsdatascience.com/l1-and-l2-regularization-methods-ce25e7fc831c

18. The **residuals** plot would indicate any trend of underestimation or overestimation. Ideally, [residual values](https://www.statisticshowto.com/residual/) should be equally and randomly spaced around the horizontal axis. Both **Mean Absolute Error** and **RMSE** would only give the error magnitude.

19. **AUC/ROC**  is the best evaluation metric for a **binary** classification model. This metric does **not** require you to set a classification **threshold**.

    For **imbalanced** datasets, you are better off using another metric called - **PR AUC** - that is also used in production systems for a highly imbalanced dataset, where the fraction of **positive class is small**, such as in case of credit card fraud detection.

20. A **"vanishing gradient"** results from multiplying together many small derivates of the **sigmoid** activation function in multiple layers. **ReLU** does not have a small derivative, and avoids this problem.

21. Fixing the **"vanishing gradient"**:

    - Multi-level heirarchy: break up levels into their own sub-networks trained individually
    - Long short-term memory(LSTM)
    - Residual Networks
      - Resnet
      - Ensemble of shorter networks
    - Better choice of activation function 
      - ReLu

22. **Transfer learning** generally involves using an existing model, or adding additional layers on top of one. 

23. A **learning rate** that is **too large** may **overshoot the true minima**, while a learning rate that is **too small will slow down convergence**.

24. **Learning rat**e affects the **speed** at which the algorithm reaches (**converges** to) the optimal weights. The SGD algorithm makes updates to the weights of the linear model for every data example it sees. The size of these updates is controlled by the learning rate.![img](https://cs231n.github.io/assets/nn3/learningrates.jpeg)

25. **Music** is fundamentally a <u>timeseries</u> problem, which **RNN's** (recurrent neural networks) are best suited for. You might see the term **LSTM** used as well, which is a specific kind of RNN.

26. **RNN** is good for: 

    - **Timeseries** data(predict future based on past; logs; where to drive the **self-driving car** based on past trajectories)
    - Data that consist of sequence of arbitrary length
      - machine translation
      - image captions
      - Machine-generated music 

27. **Custom entity recognition** extends the capability of **Amazon Comprehend** by enabling you to identify new entity types not supported as one of the preset generic entity types. This means that in addition to identifying entity types such as LOCATION, DATE, PERSON, and so on, you can **analyze documents and extract entities** like product codes or business-specific entities that fit your particular needs.

28. To **get inferences for an entire dataset**, use **batch transform**. With batch transform, you create a batch transform job using **a trained model and the dataset**, which must be stored in Amazon **S3**. Amazon SageMaker saves the inferences in an S3 bucket that you specify when you create the batch transform job.

    You can use Amazon SageMaker Batch Transform to <u>**exclude attributes**</u> before running predictions. You can also join the prediction results with partial or entire input data attributes when using data that is in **CSV, text, or JSON** format. This eliminates the need for any additional pre-processing or post-processing and accelerates the overall ML process.

29. Two methods of deploying a model for **inference**:

    - Amazon SageMaker **Hosting Services**
      - Provides a persistent **HTTPS** endpoint for getting predictions one at a time.
      - Suited for web applications that need **sub-second latency response.**
    - Amazon SageMaker **Batch Transform**
      - Doesn’t need a persistent endpoint
      - Get inferences for an **entire** dataset

30. **IP Insights** algorithm supports **only CSV** file type as training data.

31. In **XGBoost**,`subsample` prevents overfitting. 

    `eta` step size shrinkage, prevent overfitting

    `gamma`: minimum loss reduction to create a partition; larger = more conservation 

    `alpha` L1 regularization = more conservation

    `lambda` L2 regularization= more conservation

    `eval_metric` optimize on AUC, errer, rmse...

    `scale_pos_weight` Adust balance of positive and negative weights; helpful for unbalanced classes; might set to sum(negative class)/sum(positive classes)

    `max_depth` maximum depth of the tree; too high and you may overfit

    Other XGBoost hyperparameters: https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost_hyperparameters.html

32. **XGBoost Regularization:**	

    **`alpha`:** L1 regularization. Default 0; 

    **`lambda`:** L2 regularization, default 1.

33. **Boosting** generally **yields better accuracy**

    **Bagging** avoids **overfitting**, and **easier to parallize** 

34. **Bullseyes** desmonstrates 

    ![Precision versus accuracy. The bullseye represents the true value, e.g., the true location of the object, while black dots represent measurements, e.g., the estimated 3D locations of the object based on the 2D images. Source: http://www.antarcticglaciers.org/glacial-geology/dating-glacial-sediments2/precision-and-accuracy-glacial-geology/. Accessed 7.4.2016.](https://www.researchgate.net/profile/Anni-Helena-Ruotsala/publication/304674901/figure/fig6/AS:668649476067338@1536429866393/Precision-versus-accuracy-The-bullseye-represents-the-true-value-eg-the-true.ppm)

    Dart-throwing Demo

    ![2: Bias and variance in dart-throwing.](https://www.researchgate.net/profile/Syed-Akhter-6/publication/330761010/figure/fig2/AS:721145984720896@1548946009061/Bias-and-variance-in-dart-throwing.ppm)

    

35. **Batch Normalization** should **not** be a **method** of **regularization** because the main purpose of it is to **speed up the training by selecting a batch and forcing the weight to be distributed near 0**, **not** too large, **not** too small.

36. Amazon SageMaker **NTM** is an **unsupervised** learning algorithm that is used to **organize a corpus of documents into *topics* that contain word groupings based on their statistical distribution.** Documents that contain frequent occurrences of words such as "bike", "car", "train", "mileage", and "speed" are likely to share a topic on "transportation" for example. Topic modeling can be used to classify or summarize documents based on the topics detected or to retrieve information or recommend content based on topic similarities. The topics from documents that NTM learns are characterized as a ***latent representation*** because the topics are inferred from the observed word distributions in the corpus. The semantics of topics are usually inferred by examining the top ranking words they contain. Because the method is unsupervised, **only the number of topics**, not the topics themselves, are **prespecified**. In addition, the topics are not guaranteed to align with how a human might naturally categorize documents.

37. **1×1 convolutions** are called **bottleneck** structure in **CNN**, which can:

    - It suffers **less overfitting** due to **small kernel size**

    - It can be used for **feature pooling**
    - can help in **dementionality reduction** 

38. **CNN** is called "**feature-location invariant**"

39. **CNN typical usage:** 

    Conv2D -> Maxpooling2D -> Dropout -> Flatten -> Dense -> Dropout -> Softmax

40. **Softmax** usually used as the last layer of the **multiple classification** problem. It can't product more than one label(sigmoid can)

41. **Logistic activation, Sigmoid, or Soft Step** all represent the same function: Logistic (Sigmoid or Soft Step): f(x)=sigma(x)=1/(1-exp(-x))

42. **Entropy** is a measure of the **uncertainty associated with a given distribution** -- **it measures how much information is required, on average, to identify random samples from that distribution.** **Cross entropy** can be used to define a **loss function in machine learning and optimization**. Cross entropy is related to **log-likelihood**: **maximizing the likelihood is the same as minimizing the cross-entropy.**

43. **BLEU** (bilingual evaluation understudy) is an algorithm for **evaluating the quality of the text which has been machine-translated from one natural language to another.** The range of this metric is from 0.0 (a perfect translation mismatch) to 1.0 (a perfect translation match).

44. For **stochastic gradient descent**, the batch size equals **1**.

       For **batch gradient descen**t, the **batch size equals the size of the training set**.

       And, for **mini-batch gradient descent,** the batch size is greater than 1 but less than the size of the training set.

45. The small **batch size** can result:

       1) **Faster updates in the model weights**

       2) Noise and oscillations in the training process, which **might be able to escape the local minima**

46. For missing data, **Deep learning** is better suited to the imputation of categorical data. Square footage is **numerical, which is better served by kNN.**

#  ML Implementation and Operation

1. **Inference Pipeline** can be considered as an Amazon SageMaker model that you can use to make either real-time predictions or to process batch transforms directly without any external preprocessing.

2. You can use **Inference Pipeline** to package **Spark** and **scikit-learn** based preprocessors into containers:

   https://docs.aws.amazon.com/sagemaker/latest/dg/inference-pipeline-mleap-scikit-learn-containers.html

3. An **inference pipeline** is a Amazon SageMaker model that is composed of a linear sequence of **two to fifteen** containers that process requests for inferences on data. You use an inference pipeline to define and deploy any combination of pretrained SageMaker built-in algorithms and your own custom algorithms packaged in Docker containers. You can use an inference pipeline to combine preprocessing, predictions, and post-processing data science tasks. Inference pipelines are fully managed.

4. Within an **inference pipeline** model, Amazon SageMaker handles **invocations** as a sequence of **HTTP** requests.

5. **Neo** currently supports image classification models exported as frozen graphs from **TensorFlow, MXNet, or PyTorch, and XGBoost** models.:

   https://docs.aws.amazon.com/sagemaker/latest/dg/neo.html

6. three advantages of using Amazon **Neo** with SageMaker models are:

   **(i) run ML models with up to 2x better performance,** 

   **(ii) reduce framework size by 10x, and**

   **(iii) run the same ML model on multiple hardware platforms.**

7. AWS **Greengrass** is not used for **code optimization**.

8. **version tag** should be used in the **Registry Paths**:

   “:1” is the correct version tag for **production** systems.

9. **SageMaker** does not support **resource based policies**. You can create a role to **delegate access** or provide access via **identity federation**. 

10. If you want Amazon **SageMaker** to replicate a subset of data on each ML compute instance that is launched for model training, specify **`ShardedByS3Key`** for **S3DataDistributionType** field.

11. There are no **inter-node communications for batch processin**g, so **inter-node traffic encryption is not required**. SSH and AWS-SSE are not used for inter-node traffic encryption.

12. If the value of the objective metric for the current training job is **worse** (higher when minimizing or lower when maximizing the objective metric) than the **median** value of running averages of the objective metric for previous training jobs up to the same epoch, Amazon SageMaker **stops the current training job.**

13. By using Amazon **Elastic Inference** (EI), you can **speed up the throughput and decrease the latency of getting real-time inferences** from your **deep learning models** that are deployed as Amazon SageMaker hosted models, but **at a fraction of the cost** of using a **GPU** instance for your endpoint.

14. **Amazon Elastic Inference** allows you to attach **low-cost GPU-powered acceleration** to <u>Amazon EC2 and Sagemaker instances or Amazon ECS tasks</u>, to **reduce the cost of running deep learning <u>inference</u> by up to 75%**. <u>Amazon Elastic Inference supports **TensorFlow, Apache MXNet, PyTorch, and ONNX models**.</u>

15. **Network isolation** is **not supported** by the following managed Amazon SageMaker containers as **they require access to Amazon S3**:

    **Chainer**

    **PyTorch**

    **Scikit-learn**

    Amazon SageMaker **Reinforcement Learning**

16. The default **Sagemaker** IAM role gets permissions to access any bucket that has `sagemaker` in the name. If you add a policy to the role that grants the SageMaker service principal **`S3FullAccess` permission, the *name of the bucket does not need to contain `sagemaker`*. ** Granting public access to S3 bucket is not recommended. 

17. **IAM** does **not allow nesting or hierarchy of groups**.

18. For the **EC2 instance**, **IAM Role** is the recommended mechanism for managing access. You can attach the required policy to the IAM Role for DynamoDB access.  **DynamoDB does not support resource-based policy.**

19. **Group** is **not considered identity**, and you **cannot grant access to a group in a resource-based policy**. With **Resource-based** policy, you can **grant access to users, roles and other accounts**.

20. **Attribute-Based Access Control (ABAC)** is an authorization strategy that **defines permissions based on attributes (or tags)**. You can structure polices to allow operations when the **principal's tag matches the resource tag**. This approach is **useful in an environment that is growing or changing rapidly.** For example, you can check the cost center of an employee with that of the resource and allow access only if the cost center's match. **RBAC**(resource based), on the other hand, requires **ongoing maintenance to update the resource list.**

21. **XGBoost** is a **CPU-only** algorithm, and won't benefit from the GPU's of a P3 or P2. It is also **memory-bound**, making M4 a better choice than C4.

22. With **Pipe** input mode, your data is fed on-the-fly into the algorithm container **<u>without involving any disk I/O</u>.** This approach shortens the **lengthy download process and dramatically reduces startup time**. <u>It also offers generally **better read throughput than File input mode**.</u> This is because your data is fetched from Amazon S3 by a highly optimized <u>multi-threaded</u> background process. It also allows you to train on datasets that are much larger than the **16 TB Amazon Elastic Block Store (EBS)** volume size limit.

    **Pipe mode enables the following:**

    \- <u>**Shorter startup**</u> times because the data is being streamed instead of being downloaded to your training instances.

    \- **Higher I/O throughputs** due to high-performance streaming agent.

    \- Virtually limitless data processing capacity.

    With Pipe mode, the startup time is reduced significantly from **11.5 minutes to 1.5 minutes** in most experiments. Also, the overall **I/O throughput is at least twice as fast as that of File mode**. Both of these improvements made a positive impact on the total training time, which is reduced by up to 35%.

    **File mode is the default mode for training a model in Amazon SageMaker.** 

23. The algorithms that support **Pipe input mode** today when used with **protobuf recordIO-encoded** datasets are <u>Principal Component Analysis (PCA), K-Means Clustering, Factorization Machines, Latent Dirichlet Allocation (LDA), Linear Learner (Classification and Regression), Neural Topic Modelling, and Random Cut Forest. AWS Mechanical Turk, Amazon Comprehend, and Amazon QuickSight are the fully managed AWS services for crowdsourcing tasks, Natural Language Processing (NLP), and Business Intelligence (BI)</u>.

24. Amazon SageMaker's **Pipe mode** does **not** support Apache **Parquet** data format.

25. You can use **Amazon CloudWatch API** operations to send the **training metrics to CloudWatch**, and create a dashboard of those metrics. Lastly, use Amazon Simple Notification Service **(Amazon SNS) to send a notification when the model is overfitting**.

26. **CloudWatch Event** does **not** capture events that occur **during model training**

27. The **`dropout`** hyperparameter refers to the dropout probability for network layers. *A* dropout is a form of regularization used in neural networks that <u>reduces overfitting by trimming codependent neurons.</u>

    This is an optional parameter in Amazon SageMaker **Object2vec**. **Increasing the value of this parameter may say solve the overfitting of the model.**

    **L1 regularization** method is **not** available to Amazon SageMaker **Object2vec**. This is used for simple regression models like a Linear learner.

28. With the **Lifecycle configuration** feature in Amazon SageMaker, you can automate these customizations to be applied at different phases of the lifecycle of an instance. For example, you can write a script to install a list of libraries and, using the Lifecycle configuration feature, configure the scripts to **automatically execute every time your notebook instance is started**. Similarly, you can choose to **automatically run the script only once when the notebook instance is created.**

29. The performance of deep learning neural networks often improves with the amount of data available.

    **Data augmentatio**n is a technique to artificially create new training data from existing training data. <u>This is done by applying domain-specific techniques to examples from the training data that create new and different training examples.</u>

    **Image data augmentation** is perhaps the most well-known type of data augmentation and involves **creating transformed versions of images** in the training dataset that belong to the same class as the original image.

30. **T-SNE** is used to preprocess a dataset that contains highly correlated variables.

31. **Apache Spark** is a **fast and general engine for large-scale data processing**. Amazon EMR features Amazon **EMR runtime for Apache Spark**, a performance-optimized runtime environment for Apache Spark that is active by default on Amazon EMR clusters.

32. **Apache Spark is a distributed processing framework and programming model that helps you do machine learning, stream processing, or graph analytics using Amazon EMR clusters.** 

    **Apache ZooKeeper** is a **centralized service for maintaining configuration information** and **providing distributed synchronization** to distributed applications.

    **Apache MXNet** is an **open-source, deep learning framework.**

    **Apache Pig** is more suitable for running **big data analysis**.

33. Integrating **SageMaker and Spark:**

    - **Pre-process data as normal with Spark** - Generate DataFrames
    - Use **SageMaker-spark** library 
    - **SageMakerEstimator - K-means, PCA, XGBoost**
    - SageMakerModel 

    ##### How does it work?

    - Connect notebook to a remote **EMR cluster running Spark( or Zeppelin)**
    - Training df should have :
      - a feature col that is a vector of doubles 
      - An optional labels col of doubles 
    - Call **fit** on your **SageMakerEstimator** to get a **SageMakerModel**
    - Call **transform** on the SageMakerModel to make inferences
    - Works with Spark **pipelines** as well.

34. **Spark MLLib**

    - Classification: logistic regression, naive bayes

    - regression
    - decision tree
    - recommendation engine(ALS)
    - Clustering(k-means)
    - LDA
    - ML workflow utilities (pipeline, feature transformation, persistence)
    - SVD, PCA, statistics

35. With Amazon **Polly's** custom **lexicons** or **vocabularies**, you can **modify the pronunciation of particular words**, such as company names, acronyms, foreign words, and neologisms (e.g., "ROTFL", "C’est la vie" when spoken in a non-French voice). To customize these pronunciations, you upload an **XML file with lexical entries**. For example, you can customize the pronunciation of the Filipino word: "Pilipinas" by using the `phoneme` element in your input XML.

36. Lex can handle **both speech-to-text and handling the chatbot logic**. The output from Lex could be read back to the customer using Polly. Under the hood, more services would likely be needed as well to support Lex, such as Lambda and DynamoDB.

37. **Amazon SageMaker** enables you to **test multiple models or model versions** behind the **same endpoint** using **production** variants. Each **`ProductionVariant`** identifies an ML model and the resources deployed for hosting the model. You can distribute endpoint invocation requests across multiple production variants by providing the traffic distribution for each variant or invoking a variant directly for each request. 

38. SageMaker **Debugger**:

    supported framework &algos:

    - tensorflow
    - pytorch
    - MXNet
    - XGBoost
    - SageMaker generic estimator(for use with custom training containers)

39. you can create and run a custom ETL job in **AWS Glue** to <u>**redact sensitive information**</u> within a dataset stored in Amazon **S3.**

40. The ML **algorithm** should be available in **/opt/program** directory of the Docker container. Three main files there are **train, serve, and predictor.py.** <u>***train/***</u> contains the <u>logic for training the model and storing the trained model</u>. If the train file runs successfully, it will save a model to **/opt/ml/model** directory. ***serve/*** essentially runs the **logic written in predictor.py.**

    Two other files, nginx.config (NGINX configuration file) and wsgi.py (a simple wrapper file), usually can be left unchanged.

41. Amazon Sagemaker provides prebuilt Amazon SageMaker **Docker Images for TensorFlow, MXNet, Chainer, and PyTorch, as well as Docker Images for Scikit-learn and Spark ML**.

42. Your **inference** container responds to port **8080**, and must respond to ping requests in under **2 seconds.** Model artifacts need to be compressed in **tar** format, not zip.

43. Amazon SageMaker runs training jobs in an Amazon Virtual Private Cloud (Amazon **VPC**) to help keep your data secure. <u>If you are using **distributed ML frameworks and algorithms**, and your algorithms transmit information that is directly related to the model (eg weights),</u> you can provide an **additional level of security by enabling inter-container traffic encryption**. But note that turning on inter-container traffic encryption can **increase training time**, and therefore the **cost**.

44. Amazon **Macie** is a **security service** that uses **machine learning to automatically discover, classify, and protect sensitive data in AWS**. Amazon Macie recognizes sensitive data such as personally identifiable information (PII) or intellectual property and provides you with dashboards and alerts that give visibility into how this data is being accessed or moved. The fully managed service continuously monitors data access activity for anomalies and generates detailed alerts when it detects the risk of unauthorized access or inadvertent data leaks. Amazon Macie is available to protect data stored in Amazon S3.

45. Multi-factor authentication (**MFA**) could be used with **each** account for **enhanced data security**. 

46. AWS managed SageMaker policies that can be attached to the users are **AmazonSageMakerReadOnly, AmazonSageMakerFullAccess, AdministratorAccess, and DataScientist.**

47. The containers must be **NVIDIA-Docker** compatible and contain only **CUDA** toolkit, **without NVIDIA Drivers**. The toolkit includes a container runtime library and utilities to automatically configure containers to leverage NVIDIA GPUs. 

    

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

- Training job copies entire dataset from S3 to training instance beforehand
- Space Needed: Entire data set + Final model artifacts
- slower than Pipe mode
- used for incremental training 

**Pipe Mode**: 

- Training job streams data from S3 to training instance 
- Faster start time and Better Throughput 
- Space Needed: Final model artifacts
- You **MUST** use protobuf RecordIO as your training data format before you can take advantage of the Pipe mode.

### Data Format in SageMaker

#### Training Data Format

CSV

RecordIO: Data types needs to be int32, float 32, float 64

Algorithm Specipic formats( LibSVM, JSON, Parquet)

Data needs to be stored in S3

| ContentTypes for Built-in Algorithms |                                                              |
| :----------------------------------- | :----------------------------------------------------------- |
| ContentType                          | Algorithm                                                    |
| application/x-image                  | Object Detection Algorithm, Semantic Segmentation            |
| application/x-recordio               | Object Detection Algorithm                                   |
| application/x-recordio-protobuf      | Factorization Machines, K-Means, k-NN, Latent Dirichlet Allocation, Linear Learner, NTM, PCA, RCF, Sequence-to-Sequence |
| application/**jsonlines**            | BlazingText, DeepAR                                          |
| image/**jpeg**                       | Object Detection Algorithm, Semantic Segmentation            |
| image/**png**                        | Object Detection Algorithm, Semantic Segmentation            |
| text/**csv**                         | IP Insights, K-Means, k-NN, Latent Dirichlet Allocation, Linear Learner, NTM, PCA, RCF, XGBoost |
| text/**libsvm**                      | XGBoost                                                      |

#### Infetence Format

CSV

JSON

RecordIO

{{< image src="/1.png"  position="center" style="border-radius: 8px;" >}}

## SageMaker Build-in Algos

#### **BlazingText**

Unsupervised -> **word2vec**

- called *word embeding*
- has multiple modes:
  - Chow(continuous bag of words)
  - Skip-gram
  - Batch-skin-gram

**Supervised** -> multiclass, multilebel classification

- It'a useful for NLP, but it is not a NLP algo.

##### Data Type

- **Superised** mode(text classification): one sentence per line; first word is the string `_label_` followed by the label.
- word2vec wants a text file with **one training sentence per line.**

##### Hyperparameters

- Word2vec:
  - `mode`( `batch_skipgram`, `skipgram`, `cbow`)
  - `learning_rate`
  - `window_size`
  - `vector_dim`
  - `negative_samples`
- Text Classification
  - `epochs`
  - `learning_rate`
  - `word_ngrams`
  - `vector_dim`

##### Instance Types

- For cbow and skip gram: any single CPU or GPU works (single ml.p3.2xlarge)
- For batch_skipgram, can use **single or multi** CPU
- For text classification: C5 if less than 2GB training data. For larger datasets, use a single GPU

#### **Object2Vec**

Supervised -> Classification, Regression 

Unsupervised 

##### Data Types

data must be tokenized into integers; training data consists of pairs/sequence of tokens: sentence - sentence, labels - sentences, customer - customer, product - product, user - item

##### Hyperparameters

- Usual ones
- `enc1-network`, `enc2_network` - choose hcnn, bilstm, pooled_embedding...

##### Instance Type

- can only train on a single machine( GPU or CPU)
- Inference: use GPUs(ml.p2.2xlarge). Use `INFERENCE_PREDDERED_MODE`to optimize for encoder embeddings rather than clssification or regression 



#### **Factorization Machines**

Supervised -> Classification, Regression

Dealing with sparse data

- Click prediction 
- Item recommendations 

Limited tp pair-wise interactions: user -> items for example 

Can use to predict following things gavin a matrix representing some pairs of things( users & items )

- classification( click or not? Purchase or not? )
- Value(predicted rating)

Usually used in the context of recommender system 

##### Data Types

- recordIO-protobuf with float32
- sparse data means CSV **isn't** practical 

##### Instance Type

- GPU or CPU
- CPU recommended 
- GPU only works with dense data  

#### 

#### **K-Nearest Neighbors, KNN**

Supervised -> Classification, Regression

##### Data Types

recordIO-protobuf or CSV, first column is label 

File or Pipe mode on either 

##### Hyperparameters

- K!
- `sample_size`

##### Instance Type

- Training on CPU or GPU
- Inference:
  - CPU for lower latency
  - GPU for higher throughput on large batches

#### **Linear Learner**

Supervised -> Classification, Regression

##### Data Types

recordio-protobuf float32, csv

##### Processsing

- Training data must be *normalized*(all features weighted the same), 
- input data should be *shuffled*

##### Training

- uses stochastic gradient descent
- Choose an optimization algo (Adam, Adagrad, SGD,...)
- Multiple models are optimized in parallel
- Tune L1, L2 regularization

##### Validation 

- most optimal model is selected

##### Hyperparameters

- `Balance_mulyiclass_weights`
  - Gives each class equal importance in loss functions 
- `Learning_rate`, `mini_batch_size`
- `L1`
- `wd`
  - Weight decay (L2 Regularization)

##### Instance Types

- single or multiple-machine CPU or GPU 

#### **XGBoost**

Supervised -> Classification, Regression

##### Data Types

Csv, libsvm, recordIO-protobuf, parquet

##### Instance Types

- Use **CPUs only for multiple** instance training; memory-bound, not compute bound; M5 is a good choice.
- Use GPUs for single-instance training; like P3; must set `tree_method` to `gpu_hist`

#### **DeepAR**

Supervised -> Timeseries Forecasting 

Uses RNN's

Allows you to train the same model over several related timeseries.

Find frequencies and seasonality. 

##### Data Types

- JSON lines format(GZIP or Parquet)
- Each record must contain:
  - start: the starting timestamp
  - target: the timeseries values 
- Each record can contain:
  - `Dynamic_feat`: dynamic features
  - `ᓚᘏᗢ` categorical features

##### Hyperparameters

- `Contaxt_length` number of time points the model sees before making a prediction; can be smaller than seasonalities -  the model will lag one year anyhow.
- `epoches`
- `mini_batch_size`
- `learning_rate`
- `num_cells`

##### Instance Types

- Can use CPU or GPU, single or multi machine 
- CPU only for inference

#### **Object Detection**

Supervised -> Classification 

Takes in images, output all instances of objects with categories and confidence scores 

Transfer learning mode/ incremental training.

##### Data Types

- RecordIO or image format (jpg, png)
- with image format, supply a JSON file for annotation data for each image 

##### Hyperparameters

- Usual ones
- Optimizer ( sgd, adam, rmsprop, adadelta...)

##### Instance Type

- Use GPU for training
- Use CPU or GPU for inference 



#### **Image Classification**

Supervised -> Classification 

**What objects are in the image**

Resnet CNN under the hood.

Full training mode: network initialized with random weights 

**Transfer learning** mode: initialized with pre-trained weights; the top fully-connected layer is initialized with random weights 

##### Data Types

- Apache MXNet RecordIO (Not PRotobuf)
- Raw jpg or png
- image format requires `.lst` files to associate image index, class label and path to the image
- Augmented Manifest Image Format enables **Pipe** Mode

##### Hyperparameters

- Usual ones
- Optimizer ( weight decay, beta1, beta2, eps, gamma...)

##### Instance Type

- Use GPU for training
- Use CPU or GPU for inference 



#### **Semantic Segmentation**

Supervised -> Classification 

it can detect objects in an image, shape of each object along with location and **pixels** that are part of the object. 

Useful for self-driving vehicles.

##### Data Types

- jpg images and png annotations 
- jpg images accepted for inference 
- Label maps to describe annotations
- Augmented Manifest Image Format enables **Pipe** Mode

##### Hyperparameters

- Usual ones
- blackbone 

##### Instance Type

- Only **GPU** for training on a **single** machine only 
- Use CPU or GPU for **inference** 



#### **Seq2Seq**

Supervised -> Convert seq of tokens to another seq to tokens

Seq2Seq algorithm is used for text summarization – It accepts a series of tokens as input and outputs another sequence of tokens.

##### Data Types

RecordIO-protobuf(**must be int**), start with tokenized text files 

Must provide training, validation and vocabulary data.

##### Hyperparameters

- `batch_size`
- `optimizer_type`(Adam,sgd, rmsprop)
- `learning_rate`
- `num_layers_encoder`, `num_layers_decoder`
- Can optimize on:
  - accuracy(vs provided validation dataset)
  - **BLEU score**(compares against multiple reference translations)
  - **Perplexit**y(cross-entropy)

##### Instance Types

- Can only use GPU instance types.

- Can only use a **single** machine for training, bur can use multi-GPS's on one machine 

#### **K-Means**

Unsupervised -> clsutering 

##### Data Types

- Two data channels: `train` is required, `test` optional 
  - Train `sharedByS3Key`, test `FullyReplicated`
- recordIO-protobuf or CSV
- File or Pipe on either 

##### Hyperparameters

- `K!`
  - Plot within-cluster sum of squares as function of K
  - Use `elbow method`
  - optimize for tightness of clusters 
- `mini_batch_size`
- `extra_center_factor`
- `init_method`

##### Instance Type

- CPU recommended

#### **LDA**

Unsupervised -> Topic Modeling (Document level)

need to Define how many topics you want CPU based

##### Data Types

- Two data channels: `train` is required, `test` optional 
- recordIO-protobuf or CSV
- words must be tokenized into int. 
  - every document must contain a count for every word in the vocab in csv 
- Pipe mode only supported with recordIO

##### Hyperparameters

- `num_topics`
- `alpha0`
  - initial guess for concentration parameter
  - smaller values generate sparse topic mixtures 

##### Instance Type

- Single instance CPU training 

#### **Neural Topic Model(NTM)**

Unsupervised -> Topic modeling, similiar to LDA

need to Define how many topics you want 

##### Data Types

- Four data channels: `train` is required, `validation`, `test`, `auxiliary` optional 
- recordIO-protobuf or CSV
- words must be tokenized into int. 
  - every document must contain a count for every word in the vocab in csv 
  - the `auxiliary` channel is for the vocab.
- File or Pipe mode 

##### Hyperparameters

- **lowering `mini_batch_size` and `learning_rate`** can reduce validation loss  - at expense of training time  
- `num_topics`

##### Instance Type

GPU or CPU

- GPU recommended for training 
- CPU ok for inference 



#### **PCA: Principal Component Analysis**

Unsupervised -> Dimensioality reduction 

##### Data Types

- recordIO-protobuf or CSV
- File or Pipe mode on either 

##### Hyperparameters

- `Algorithm_mode`
- `subtract_mean`

##### Instance Type

- GPU or CPU 

#### **Random Cut Forest**

Unsupervised -> **anomaly detection** 

Data is sampled randomly

shows up in Kinesis Analytics as well, it works on streaming data too.

##### Data Types

- RecordIO-protobuf or CSV
- Can use File or Pipe mode on either

##### Hyperparameters

- `num_trees`: increasing reduces noise
- `num_samples_per_tree`: should be chosen such that 1/num_samples_per_tree appox the ratio of anomalous to normal data 

##### Instance Type

- **CPU** for training 
- Use CPU **inference** ( ml.c5.xl)



#### **IP Insights**

Unsupervised -> Detect unusual network activity 

automatically generates negative samples during training by random pairing entites and IPs 

##### Data Types

- CSV only - entity and IP

##### Hyperparameters

- `num_entity_vectors`:  
  - Hash size
  - set to twice the number of unique entity identifiers 
- `vectoe_dim`: 
  - size of embedding vectors
  - Scales model size
  - too large results in overfitting 
- Epochs learning_rate, batch_size, ...

##### Instance Type

- CPU or GPU
- GPU recommended 
- can use multiple GPUs
- size of CPU instance depends on `vector_dim` and `num_entity_vectors`

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

## PCA

**Normalization:** The normalization transformer normalizes numeric variables to have a mean of zero and variance of one.

### PCA on SageMaker 

#### Two Modes 

- **Regular** - Good for Sparse Data and Moderate sized datasets 

- **Random** - Good for very large datasets – uses approximation algorithm



## Factorization Machine 

Factorization Machine algorithm is optimized for handling **high dimensional sparse** datasets.

Supports Regression and Classification. 

Personalize Content - “predict” ratings/likeness 

- Click Prediction for Ad-Placement 
- Product recommendation for user 
- Movie recommendation 
- News/Social Media Feed personalization for users

### Factorization Machine – Data Format 

**Input**: recordio-protobuf (with Float32 values) 

Inference: json recordio-protobuf

## Random Cut Forest

- Unsupervised algorithm to detect outliers or anomalous data points 
- Tree based ensemble method 
- Support for Timeseries data 
- Assigns an anomaly score for each data point

### RCF Uses 

- Traffic spike due to rush hour or accident 
- DDoS attack detection 
- Unauthorized data transfer detection





## Kinesis Streams vs Firehose

### Kinesis Data Streams

-  we can write custom code for the producer and the consume
- It is going to be real time,  between 70 milliseconds and 200-millisecond latency, 
- **you must manage to scale yourself** so if you want **more throughpu**t you need to think to do something called **Shard splitting,** which means **adding Shards,** and if you want **less throughput** you need to do **Shard merging**, which is **removing** Shards. And that is quite painful to do.
- Data storage for your stream, between 1 to 7 days, and this allows  replay capability,  multiple consumer 

### Firehose

- is a delivery service.
- It's an ingestion service, so remember that word "**ingestion**". And so it's **fully** **managed** and you can send it out to Amazon **S3, Splunk, Redshift, and ElasticSearch**.
- It is fully serverless, that means we don't manage anything, **and you can do also serverless data transformations using Lambda.**

- It's going to be near real-time, that means that it's going to deliver data with the **lowest buffer time of 1 minute into your targets,**

- there is **automated scaling**, we don't need to provision capacity in advance,

- and there is no data stored so there is no replay capability.



### Kinesis Data Analytics 

Random cut forest vs Hotspot

![image-20210830031653002](/Users/flora/Library/Application Support/typora-user-images/image-20210830031653002.png)

### ![image-20210830034513019](/Users/flora/Library/Application Support/typora-user-images/image-20210830034513019.png)Kinesis Summary 

- Kinesis Data Stream: create real-time ML apps.
- Kinesis Data Firehose: ingest massive data near real-time.
- Kinesis Data Analytics: Real time ETL/ML algos on streams
- Kinesis Video Stream: real-time video stream to create ML apps.



![image-20210830223358178](/Users/flora/Library/Application Support/typora-user-images/image-20210830223358178.png)