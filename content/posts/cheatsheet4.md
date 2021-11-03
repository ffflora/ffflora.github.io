---
title: "AWS Machine Learning Specialty Cheatsheet(4)"
date: 2021-11-03T14:47:21-07:00
draft: false
toc: false
images:
tags:
- AWS
- Machine Learning
categories:	
- Data Science
---

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

17. **IAM** does **not allow nesting or hierarchy of groups**.(Can't be a child of other IAM groups.) 

18. For the **EC2 instance**, **IAM Role** is the recommended mechanism for managing access. You can attach the required policy to the IAM Role for DynamoDB access.  **DynamoDB does not support resource-based policy.**

19. **Group** is **not considered identity**, and you **cannot grant access to a group in a resource-based policy**. With **Resource-based** policy, you can **grant access to users, roles and other accounts**. Resource-based policy is also called **bucket-policy**, as the policy is attached to the S3 buckets. Resource-based policy needs to have **Principal** tag.

20. There are two types of policies: **Inline(Embeded) policy(not reusable)** and **Managed policy(reusable).** 

21. **Attribute-Based Access Control (ABAC)** is an authorization strategy that **defines permissions based on attributes (or tags)**. You can structure polices to allow operations when the **principal's tag matches the resource tag**. This approach is **useful in an environment that is growing or changing rapidly.** For example, you can check the cost center of an employee with that of the resource and allow access only if the cost center's match. **RBAC**(resource based), on the other hand, requires **ongoing maintenance to update the resource list.**

22. In SageMaker, the **`iam:PassRole`** action is needed for the Amazon SageMaker action **`sagemaker:CreateModel`**. This allows the user to pass authorization to SageMaker to actually create models.

23. **XGBoost** is a **CPU-only** algorithm, and won't benefit from the GPU's of a P3 or P2. It is also **memory-bound**, making M4 a better choice than C4.

24. With **Pipe** input mode, your data is fed on-the-fly into the algorithm container **<u>without involving any disk I/O</u>.** This approach shortens the **lengthy download process and dramatically reduces startup time**. <u>It also offers generally **better read throughput than File input mode**.</u> This is because your data is fetched from Amazon S3 by a highly optimized <u>multi-threaded</u> background process. It also allows you to train on datasets that are much larger than the **16 TB Amazon Elastic Block Store (EBS)** volume size limit.

    **Pipe mode enables the following:**

    \- <u>**Shorter startup**</u> times because the data is being streamed instead of being downloaded to your training instances.

    \- **Higher I/O throughputs** due to high-performance streaming agent.

    \- Virtually limitless data processing capacity.

    With Pipe mode, the startup time is reduced significantly from **11.5 minutes to 1.5 minutes** in most experiments. Also, the overall **I/O throughput is at least twice as fast as that of File mode**. Both of these improvements made a positive impact on the total training time, which is reduced by up to 35%.

    **File mode is the default mode for training a model in Amazon SageMaker.** 

25. The algorithms that support **Pipe input mode** today when used with **protobuf recordIO-encoded** datasets are <u>Principal Component Analysis (PCA), K-Means Clustering, Factorization Machines, Latent Dirichlet Allocation (LDA), Linear Learner (Classification and Regression), Neural Topic Modelling, and Random Cut Forest. AWS Mechanical Turk, Amazon Comprehend, and Amazon QuickSight are the fully managed AWS services for crowdsourcing tasks, Natural Language Processing (NLP), and Business Intelligence (BI)</u>.

26. Amazon SageMaker's **Pipe mode** does **not** support Apache **Parquet** data format.

27. **EC2** Storages:

    - **Instace Store(Block)**:
      - Store of host computer is **assigned** to **EC2** instance.
      - **Temporary** Storage
      - **Highest** Performance
      - Storage included as part of instance pricing 
      - **Durability**:
        - Data persists only for the **lifetime** of the instance
        - **Reboot** - Data **Persists**
        - Data is **lost** - when underlying hardware **fails**, instance **stops**, or instance **terminates**.
    - **Elastic Block Store(EBS):**
      - EBS is a managed **block** storage service
      - Storage volume is **outside** of host computer - **long term** persistance.
      - EC2 instance use EBS storage volume as a block device
      - You need to pay for allocated EBS storage
      - **Benefit:**
        - **Stop-start** EC2 instance 
        - **Persist** EBS volumes for **terminated** instances
        - **Detach and attach volume t**o a different instance in the **same availability** zone
        - Built-in **Snapshot** capability for incremental backup to S3
        - Create **AMI** from snapshots to launch new EC2 instances  

28. You can use **Amazon CloudWatch API** operations to send the **training metrics to CloudWatch**, and create a dashboard of those metrics. Lastly, use Amazon Simple Notification Service **(Amazon SNS) to send a notification when the model is overfitting**.

29. **CloudWatch Event** does **not** capture events that occur **during model training**

30. **CloudWatch Logs** will typically contain the **statistics** as reported by the **inference algorithm**. In the case of **Linear Learner**, the **predicted_label** and the **score** is returned and stored in the **CloudWatch Log entries**.

31. The **`dropout`** hyperparameter refers to the dropout probability for network layers. *A* dropout is a form of regularization used in neural networks that <u>reduces overfitting by trimming codependent neurons.</u>

    This is an optional parameter in Amazon SageMaker **Object2vec**. **Increasing the value of this parameter may say solve the overfitting of the model.**

    **L1 regularization** method is **not** available to Amazon SageMaker **Object2vec**. This is used for simple regression models like a Linear learner.

32. With the **Lifecycle configuration** feature in Amazon SageMaker, you can automate these customizations to be applied at different phases of the lifecycle of an instance. For example, you can write a script to install a list of libraries and, using the Lifecycle configuration feature, configure the scripts to **automatically execute every time your notebook instance is started**. Similarly, you can choose to **automatically run the script only once when the notebook instance is created.**

33. The performance of deep learning neural networks often improves with the amount of data available.

    **Data augmentatio**n is a technique to artificially create new training data from existing training data. <u>This is done by applying domain-specific techniques to examples from the training data that create new and different training examples.</u>

    **Image data augmentation** is perhaps the most well-known type of data augmentation and involves **creating transformed versions of images** in the training dataset that belong to the same class as the original image.

34. **T-SNE** is used to preprocess a dataset that contains highly correlated variables.

35. **Apache Spark** is a **fast and general engine for large-scale data processing**. Amazon EMR features Amazon **EMR runtime for Apache Spark**, a performance-optimized runtime environment for Apache Spark that is active by default on Amazon EMR clusters.

36. **Apache Spark is a distributed processing framework and programming model that helps you do machine learning, stream processing, or graph analytics using Amazon EMR clusters.** 

    **Apache ZooKeeper** is a **centralized service for maintaining configuration information** and **providing distributed synchronization** to distributed applications.

    **Apache MXNet** is an **open-source, deep learning framework.**

    **Apache Pig** is more suitable for running **big data analysis**.

37. Integrating **SageMaker and Spark:**

    - **Pre-process data as normal with Spark** - Generate DataFrames
    - Use **SageMaker-spark** library // [sagemaker-spark-sdk](https://github.com/aws/sagemaker-spark/blob/master/sagemaker-spark-sdk) 
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

38. **Spark MLLib**

    - Classification: logistic regression, naive bayes

    - regression
    - decision tree
    - recommendation engine(ALS)
    - Clustering(k-means)
    - LDA
    - ML workflow utilities (pipeline, feature transformation, persistence)
    - SVD, PCA, statistics

39. With Amazon **Polly's** custom **lexicons** or **vocabularies**, you can **modify the pronunciation of particular words**, such as company names, acronyms, foreign words, and neologisms (e.g., "ROTFL", "C’est la vie" when spoken in a non-French voice). To customize these pronunciations, you upload an **XML file with lexical entries**. For example, you can customize the pronunciation of the Filipino word: "Pilipinas" by using the `phoneme` element in your input XML.

40. Lex can handle **both speech-to-text and handling the chatbot logic**. The output from Lex could be read back to the customer using Polly. Under the hood, more services would likely be needed as well to support Lex, such as Lambda and DynamoDB.

41. **Amazon SageMaker** enables you to **test multiple models or model versions** behind the **same endpoint** using **production** variants. Each **`ProductionVariant`** identifies an ML model and the resources deployed for hosting the model. You can distribute endpoint invocation requests across multiple production variants by providing the traffic distribution for each variant or invoking a variant directly for each request. 

42. SageMaker **Debugger**:

    supported framework &algos:

    - tensorflow
    - pytorch
    - MXNet
    - XGBoost
    - SageMaker generic estimator(for use with custom training containers)

43. you can create and run a custom ETL job in **AWS Glue** to <u>**redact sensitive information**</u> within a dataset stored in Amazon **S3.**

44. The ML **algorithm** should be available in **/opt/program** directory of the Docker container. Three main files there are **train, serve, and predictor.py.**

    <u>***train/***</u> contains the <u>logic for training the model and storing the trained model</u>. 

    If the train file runs successfully, it will save a model to **/opt/ml/model** directory. Deployment code for inference goes here.

    **/opt/ml/code** training script files goes here.

    ***serve/*** essentially runs the **logic written in predictor.py.**

    Two other files, nginx.config (NGINX configuration file) and wsgi.py (a simple wrapper file), usually can be left unchanged.

45. Amazon Sagemaker provides prebuilt Amazon SageMaker **Docker Images for TensorFlow, MXNet, Chainer, and PyTorch, as well as Docker Images for Scikit-learn and Spark ML**.

46. When using custom **TensorFlow** code, the Amazon SageMaker **Python SDK** supports **script mode** training scripts. 

47. Your **inference** container responds to port **8080**, and must respond to ping requests in under **2 seconds.** Model artifacts need to be compressed in **tar** format, not zip.

48. Amazon SageMaker runs training jobs in an Amazon Virtual Private Cloud (Amazon **VPC**) to help keep your data secure. <u>If you are using **distributed ML frameworks and algorithms**, and your algorithms transmit information that is directly related to the model (eg weights),</u> you can provide an **additional level of security by enabling inter-container traffic encryption**. But note that turning on inter-container traffic encryption can **increase training time**, and therefore the **cost**.

49. VPC is within AWS customer account, SageMaker/EBS/... are within AWS service account. 

50. Amazon **Macie** is a **security service** that uses **machine learning to automatically discover, classify, and protect sensitive data in AWS**. Amazon Macie recognizes sensitive data such as personally identifiable information (PII) or intellectual property and provides you with dashboards and alerts that give visibility into how this data is being accessed or moved. The fully managed service continuously monitors data access activity for anomalies and generates detailed alerts when it detects the risk of unauthorized access or inadvertent data leaks. Amazon Macie is available to protect data stored in Amazon S3.

51. Multi-factor authentication (**MFA**) could be used with **each** account for **enhanced data security**. 

52. AWS managed SageMaker policies that can be attached to the users are **AmazonSageMakerReadOnly, AmazonSageMakerFullAccess, AdministratorAccess, and DataScientist.**

53. The containers must be **NVIDIA-Docker** compatible and contain only **CUDA** toolkit, **without NVIDIA Drivers**. The toolkit includes a container runtime library and utilities to automatically configure containers to leverage NVIDIA GPUs. 

54. When **scaling** responsiveness is not as fast as you would like, you should look at the **cooldown** period. The cooldown period is a duration when scale events will be ignored, allowing the new instances to become established and take on load. Decreasing this value will launch new variant instance faster.

55. Amazon **ECR** is **integrated** with Amazon **ECS** allowing you to easily store, run, and manage **container images for applications running on Amazon ECS**. All you need to do is specify the Amazon ECR repository in your task definition and Amazon ECS will retrieve the appropriate images for your applications.

---





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
-  It is going to be real time,  between 70 milliseconds and 200-millisecond latency, 
-  **you must manage to scale yourself** so if you want **more throughpu**t you need to think to do something called **Shard splitting,** which means **adding Shards,** and if you want **less throughput** you need to do **Shard merging**, which is **removing** Shards. And that is quite painful to do.
-  Data storage for your stream, between 1 to 7 days, and this allows  replay capability,  multiple consumer 

### Firehose

- is a delivery service.
- It's an ingestion service, so remember that word "**ingestion**". And so it's **fully** **managed** and you can send it out to Amazon **S3, Splunk, Redshift, and ElasticSearch**.
- It is fully serverless, that means we don't manage anything, **and you can do also serverless data transformations using Lambda.**

- It's going to be near real-time, that means that it's going to deliver data with the **lowest buffer time of 1 minute into your targets,**

- there is **automated scaling**, we don't need to provision capacity in advance,

- and there is no data stored so there is no replay capability.



### Kinesis Summary 

- Kinesis Data Stream: create real-time ML apps.
- Kinesis Data Firehose: ingest massive data near real-time.
- Kinesis Data Analytics: Real time ETL/ML algos on streams
- Kinesis Video Stream: real-time video stream to create ML apps.
- Kinesis Agent: is a stand-alone Java software application that offers an easy way to collect and send data to Kinesis Streams and Kinesis Firehose. The agent continuously monitors a set of files and sends new data to your stream. The agent handles file rotation, checkpointing, and retry upon failures. It delivers all of your data in a reliable, timely, and simple manner. It also emits Amazon CloudWatch metrics to help you better monitor and troubleshoot the streaming process



