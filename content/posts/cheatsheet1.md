---
title: "AWS Machine Learning Specialty Cheatsheet(1)"
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

1. In <u>Kinesis Data Stream</u>, number_of_shards = max (incoming_write_bandwidth_in_KB/**1000**, outgoing_read_bandwidth_in_KB/**2000**)

   where

   incoming_write_bandwidth_in_KB = average_data_size_in_KB multiplied by the number_of_records_per_seconds. 

   outgoing_read_bandwidth_in_KB = incoming_write_bandwidth_in_KB multiplied by the number_of_consumers.

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

    Amazon ElastiCache is in-memory data stores in the cloud that cannot be connected to Glue. (A caveat: a user can write custom Scala or Python code and import custom libraries and Jar files into Glue ETL jobs to access data sources not natively supported by AWS Glue, like ElastiCache). When you are creating a new Job on the console, you can **specify** one or more **library** ***`.zip`*** files by choosing **Script Libraries** and job parameters (optional) and **entering the full Amazon S3** library path(s) in the same way you would when creating a development endpoint.

    https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-libraries.html

15. **Kinesis Data Firehose is used for streaming data scenarios**. **AWS Glue ML Transforms job can perform deduplication** in a serverless fashion.

16. **Amazon Kinesis Data Firehose** **<u>*buffers incoming streaming*</u>** data to a certain size or for a certain period of time before delivering it to destinations. You can configure the buffer size and buffer interval while <u>creating your **delivery stream**.</u>

    Buffer size is in MBs and ranges from <u>1MB to 128MB</u> for Amazon S3 destination and <u>**1MB to 100MB**</u> for Amazon Elasticsearch Service destination. Buffer interval is in seconds and ranges from **60 seconds to 900 seconds**. Please note that in circumstances where data delivery to destination is falling behind data writing to a delivery stream, Firehose raises buffer size dynamically to catch up and make sure that all data is delivered to the destination.

    ![img](https://img-c.udemycdn.com/redactor/raw/test_question_description/2020-12-04_16-41-13-3dcfe5855fa5c240d34bedca038dcb9b.JPG)

17. Kinesis Firehose Data **Delivery Failure Handing** 

    ![](https://img-c.udemycdn.com/redactor/raw/test_question_description/2020-12-05_02-32-25-005129db5966b762cc65018c53796c0e.JPG)

18. Firehose can deliver data to **S3, Elastic Search, Splunk, Redshift, and HTTP Endpoints.** Athena is not a database, and Firehose **doesn't** deliver data to DynamoDB.

19. There is no such thing as a ~~LOAD~~ command for **Redshift**. ***COPY*** is much faster than ***INSERT***. *UNLOAD* is used to write the results of a Redshift query to one or more text files on Amazon S3.

19. There are two **in-place query** with S3: **Athena and Redshift Spectrum.**

20. - **Athena**: **ad-hoc** data discovery and **SQL** querying. 
    - **Redshift Spectrum**: more **complex** queries and **large** number of **uses**. 

21. Only **GZIP** is supported if the data is loaded to Amazon **Redshift**.

22. Amazon SageMaker **models** are stored as **`model.tar.gz`** in the S3 bucket specified in **`OutputDataConfig` `S3OutputPath`** parameter of the **`create_training_job`** call. You can specify most of these model artifacts when creating a hosting model. You can also open and review them in your notebook instance. When `model.tar.gz` is untarred, it contains `model_algo-1`, which is a serialized Apache MXNet object.

23. **Workload management (WLM)** is only available in Amazon **Redshift**.

24. If you want Amazon SageMaker to replicate a subset of data on each ML compute instance that is launched for model training, specify ***`ShardedByS3Key`*** for <u>**S3DataDistributionType**</u> field.

25. **KCL** helps you consume and process data from a **Kinesis data stream** by taking care of many of the complex tasks associated with distributed computing. These include load balancing across multiple consumer application instances, responding to consumer application instance failures, checkpointing processed records, and reacting to resharding. The KCL takes care of all of these subtasks so that you can focus your efforts on **writing your custom** record-processing logic.

26. Amazon **KPL** is only used for <u>streaming data into Amazon Kinesis Data Streams</u> and not for reading data from it.

27. **Kinesis Product Library**(KPL) provides built-in performance benefits and ease of use advantages. 

    - **Performance Benefits**

      The KPL can help build high-performance producers. 

    - **Consumer-Side Ease of Use**

      For consumer-side developers using the KCL in Java, the KPL integrates without additional effort. 

      For consumer-side developers who do not use the KCL but instead use the API operation `GetRecords` directly, a KPL Java library is available to extract the individual user records before returning them to the user.

    - **Producer Monitoring**

      You can collect, monitor, and analyze your Kinesis Data Streams producers using Amazon CloudWatch and the KPL. 

    - **Asynchronous Architecture**

    https://docs.aws.amazon.com/streams/latest/dev/developing-producers-with-kpl.html#developing-producers-with-kpl-advantage

28. Hyperparameters should be tuned against the **Validation** Set.

29. Kinesis cheatsheet https://tutorialsdojo.com/amazon-kinesis/

30. Amazon **FSx** for **Lustre** <u>speeds up your **File mode** training jobs by serving your Amazon S3 data to Amazon SageMaker at high speeds</u>. The first time you run a training job, Amazon FSx for Lustre automatically copies data from Amazon S3 and makes it available to Amazon SageMaker. Additionally, the same Amazon FSx file system can be used for subsequent iterations of training jobs on Amazon SageMaker, preventing repeated downloads of common Amazon S3 objects. Because of this, Amazon FSx has the most benefit to training jobs that **have training sets in Amazon S3 and in workflows where training jobs must be run several times using different training algorithms or parameters to see which gives the best result.**

31. Data storage for machine learning modeling could be from **S3**, **Amazon Fsx for Lustre, and Amazon EFS.**

32. Amazon **FSx** has two modes:

    1. **Standalone** File share
    2. Link to **S3 Bucket** and access S3 as a file share

33. **AWS Database Migration Service (AWS DMS)** is a cloud service that makes it easy to migrate relational databases, data warehouses, NoSQL databases, and other types of data stores. You can use AWS DMS to migrate your data into the AWS Cloud, between on-premises instances (through an AWS Cloud setup), or between combinations of cloud and on-premises setups. With AWS DMS, you can perform one-time migrations, and you can replicate ongoing changes to keep sources and targets in sync.

    - continuous Data Replication
    - No data transformation (**ETL jobs needs to be done later somewhere else**.)
    - once the data is in AWS, you can use <u>Glue</u> to transform it. 
    - You can use Database Migration Service for one-time data migration into RDS and EC2-based databases.
    - You can also continuously replicate your data with high availability (enable multi-AZ) and consolidate databases into a petabyte-scale data warehouse by streaming data to Amazon Redshift and Amazon S3.
    - Replication between on-premises to on-premises databases is not supported.
    - The service provides an end-to-end view of the data replication process, including diagnostic and performance data for each point in the replication pipeline.
    - You only pay for the compute resources used during the migration process and any additional log storage. Each database migration instance includes storage sufficient for swap space, replication logs, and data cache for most replications and inbound data transfer is free.

34. The **S3 Standard-IA** storage class is designed for long-lived and infrequently accessed data. (IA stands for *infrequent access*.) S3 Standard-IA is available for **millisecond access** (same as the S3 Standard storage class). Amazon S3 charges a retrieval fee for these objects, so they are most suitable for infrequently accessed data.

    Amazon S3 **Glacier Deep Archive** does not provide immediate access. Expedited retrievals may still take **hours** to complete.

35. **S3** related: 

    - By default, you can create up to **100** buckets in **each** of your AWS accounts.
    - You **can’t change its Region** after creation.
    - You can host **static** websites by configuring your bucket for website hosting.
    - You **can’t delete** an S3 bucket using the Amazon S3 console if the bucket contains **100,000** or more objects. You **can’t** delete an S3 bucket using the **AWS CLI if versioning is enabled**.

36. **S3 Data Consistency Model**

    - read-after-write consistency for PUTS of new objects in your S3 bucket in all regions
    - strong consistency for read-after-write HEAD or GET requests
    - strong consistency for overwrite PUTS and DELETES in all regions
    - strong read-after-write consistency for any storage request
    - eventual consistency for listing all buckets after deleting a bucket (deleted bucket might still show up)
    - eventual consistency on propagation of enabling versioning on a bucket for the first time.

37. **S3 Select**

    - S3 Select is an Amazon S3 capability designed to **pull out only the data you need** from an object, which can dramatically improve the performance and reduce the cost of applications that need to access data in S3.
    - Amazon S3 Select works on objects stored in **CSV and JSON format, Apache Parquet format, JSON Arrays, and BZIP2 compression for CSV and JSON objects.**
    - **CloudWatch** Metrics for S3 Select lets you monitor S3 Select usage for your applications. These metrics are available at **1-minute** intervals and lets you quickly identify and act on operational issues.

38. **Encryption** in S3:

    - - **Server-side** Encryption using
        - **Amazon S3-Managed Keys (SSE-S3)**
        - **AWS KMS-Managed Keys (SSE-KMS)**
        - **Customer-Provided Keys (SSE-C)**
      - **Client-side** Encryption using
        - AWS KMS-managed customer master key
        - client-side master key

39. **Customer Master Key** provides additional layer of security and control.

40. **S3 Server-side encryption** can enable encryption settings as part of Bucket configuration, and all objects are automatically encrypted. **Client-Side encryption** is useful for scenarios where you want to encrypt and decrypt at the **client** and store the encrypted object in S3.

41. **Availability Zones** give customers the ability to operate production applications and databases that are more highly available, fault-tolerant, and scalable than would be possible from a single data center. AWS maintains multiple AZs around the world and more zones are added at a fast pace. Each AZ can be multiple data centers (typically 3), and at full scale can be hundreds of thousands of servers. They are fully isolated partitions of the AWS Global Infrastructure. With their own power infrastructure, the AZs are physically separated by a meaningful distance, many kilometers, from any other AZ, although all are within 100 km (60 miles of each other).

    All AZs are interconnected with high-bandwidth, low-latency networking, over fully redundant, dedicated metro fiber providing high-throughput, low-latency networking between AZs. The network performance is sufficient to accomplish synchronous replication between AZs. AWS Availability Zones are also powerful tools for helping build highly available applications. AZs make partitioning applications about as easy as it can be. If an application is partitioned across AZs, companies are better isolated and protected from issues such as lightning strikes, tornadoes, earthquakes and more.

42. **Durability** – S3 **replicates** data across **multiple availability zones and multiple devices in each availability zone**. **Cross-Region** Replication is used for **disaster** **recovery** by keeping a copy of your bucket in another region. **Versioning** protects from accidental and malicious deletes. **Server-Side encryption** is used for storing the data encrypted **at rest**.

43. **Amazon Athena** is an interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL. Athena is **serverless**, so there is no infrastructure to manage, and you pay only for the queries that you run.

    Athena can run queries to analyze unstructured, semi-structured, and structured data stored in S3.

44. **Kinesis** and **Athena** can feed data into **MapReduce** jobs.

45. In Kinesis Data Stream, the minimum value of a stream’s time period is 24 hours, but this can be increased up to 8760 hours (365 days).

46. **AWS Lake formation** integrates with the following services which also accepts the lake formation permissions:

    1) AWS Glue,

    2) Amazon Athena,

    3) Amazon Redshift Spectrum,

    4) Amazon Quicksight,

    5) Amazon EMR and

    6) AWS Glue DataBrew. 

47. **An EMR cluster has three nodes:**

    1) **Master** node - Runs and manages the master components of distributed applications.

    2) **Core** node - Coordinates the data storage as part of the HDFS file system with the task nodes.

    3) **Task** nodes - Optional nodes, and can be added to perform parallel computation tasks on data.

    Task nodes can be used with <u>spot EC2 instances, as they can be used and removed whenever required, and this does not affect the working of other nodes.</u>

    Using spot instances can provide a huge cost saving compared to the on-demand and reserved instances, as this spares the EC2 instances once the job is done. The master and core nodes can't be used with the spot instances, as their availability is a must requirement for the EMR cluster.

48. While you can use Spot instances on any node type, a Spot interruption on the Master node requires terminating the entire cluster, and on a Core node, it can lead to HDFS data loss.

49. **EMR Storage**

    - HDFS
    - EMRFS: access to S3 as if it were HDFS
    - Local file system 
    - EBS for HDFS

50. **<u>Kinesis Data Streams PutRecord API</u>** uses ***name of the stream***, ***a partition key*** and the ***data blob*** whereas <u>**Kinesis Data Firehose PutRecord API**</u> uses the ***name of the delivery stream*** and the ***data record***.

51. ![img](https://img-c.udemycdn.com/redactor/raw/test_question_description/2020-12-06_05-31-48-e1d0b84a350d082ddd023606a22f2432.jpg)

    Kinesis Data Firehose requires only Stream Data Name and the Data Record, whereas the Kinesis Data Stream needs ShardId, Partition Key, and the Data Record.

    Also, Kinesis Firehose scales automatically depending on the input data stream, without any need to manually increase the ingestion size.

52. Using SQS would require implementing substantial functionality on top of SQS, and AWS **Batch is only designed for scheduling and allocating the resources needed for batch processing.**

53. **AWS Data Pipeline** provides a managed orchestration service that gives you greater flexibility in terms of the execution environment, access and control over the compute resources that run your code, as well as the code itself that does data processing. AWS Data Pipeline **launches compute resources** in your account **allowing you direct access to the Amazon EC2 instances or Amazon EMR clusters.**

54. **Preventative** controls encompass the AWS **CAF**(Cloud Adoption Framework) Security Perspective capabilities of IAM, infrastructure security, and data protection. They are **(i) IAM, (ii) Infrastructure security, and (iii) Data protection (encryption and tokenization).**

    **Detective** controls detect changes to configurations or undesirable actions and enable timely incident response. They are **(i) Detection of unauthorized traffic, (ii) Configuration drift, (iii) Fine-grained audits.**

55. AWS Step Functions lets you coordinate multiple AWS services into serverless workflows so you can build and update apps quickly. Using Step Functions, you can design and run workflows that stitch together services, such as AWS Lambda, AWS Fargate, and Amazon SageMaker, into feature-rich applications. Workflows are made up of a series of steps, **with the output of one step acting as input into the next**. They are perfect to run multiple ETL jobs that hand off the input to ML workflows. **(The keyword in the question is 'workflow'.)**

56. **Kinesis Data Analytics** can use **lambda to convert GZIP** and can run SQL on the converted data. A specialist can simply select an AWS Lambda function from the Kinesis Analytics application source page in the AWS Management console. Kinesis Analytics application will automatically process raw data records using the Lambda function and send transformed data to SQL code for further processing.

57. **Object tags** are useful for **security and lifecycle management.** One can use tags to identify an object with, say PII (Personally identifiable information), or use S3 for one-dimensional object categorization. One can also add another dimension using bucket prefixes.

58. **Collaborative filtering** is based on (user, item, rating) tuples. So, unlike content-based filtering, it leverages other users’ experiences. The main concept behind collaborative filtering is that users with similar tastes (based on observed user-item interactions) are more likely to have similar interactions with items they haven't seen before.

    Compared to content-based filtering, collaborative filtering provides better results for **diversity** (how dissimilar recommended items are); **serendipity** (a measure of how surprising the successful or relevant recommendations are); and **novelty** (how unknown recommended items are to a user).

59. Amazon EMR offers you two options to work with Jupyter notebooks: (i) EMR Notebook, and (ii) JupyterHub. An EMR notebook is a 'serverless' Jupyter notebook. Unlike a traditional notebook, the contents of an EMR notebook itself — the equations, visualizations, queries, models, code, and narrative text — are saved in Amazon S3 separately from the cluster that runs the code.



