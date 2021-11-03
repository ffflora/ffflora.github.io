---
title: "AWS Machine Learning Specialty Cheatsheet(3)"
date: 2021-11-03T14:47:15-07:00
draft: false
toc: false
images:
tags:
- AWS
- Machine Learning
categories:	
- Data Science
---

# Modeling

1. ***Object2Vec*** can be used to find semantically similar **objects** such as questions. ***BlazingText Word2Vec*** can only find semantically similar **words**. 

2. **mode** is the mandatory hyperparameter for both the **Word2Vec** (unsupervised) and **Text Classification** (supervised) modes of the SageMaker **BlazingText** algorithm.

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

47. In classifications tasks with **imbalanced** class distributions, we should prefer **StratifiedKFold** over **KFold**.



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

- **Supervised** mode(text classification): one sentence per line; first word is the string `_label_` followed by the label.

- word2vec wants a text file with **one training sentence per line.**

- ###### Training and Validation Data Format for the Word2Vec Algorithm

  For **Word2Vec** training, upload the file under the ***train*** channel. No other channels are supported. The **file should contain a training sentence per line.**

- ###### Training and Validation Data Format for the Text Classification Algorithm

  For supervised mode, you can train with file mode or with the augmented manifest text format.

- Train with File Mode

  For `supervised` mode, the training/validation file should contain a training sentence per line along with the labels. Labels are words that are prefixed by the string *__label__*. Here is an example of a training/validation file:

```
__label__4  linux ready for prime time , intel says , despite all the linux hype , the open-source movement has yet to make a huge splash in the desktop market . that may be about to change , thanks to chipmaking giant intel corp .

__label__2  bowled by the slower one again , kolkata , november 14 the past caught up with sourav ganguly as the indian skippers return to international cricket was short lived .
```

​	**Note**

​	The order of labels within the sentence doesn't matter.

​	Upload the training file under the train channel, and optionally upload the validation file under the 	validation channel.

###### Train with Augmented Manifest Text Format

The supervised mode also supports the augmented manifest format, which enables you to do training in **pipe** mode without needing to create RecordIO files. While using the format, an S3 manifest file needs to be generated that contains the list of sentences and their corresponding labels. The manifest file format should be in [JSON Lines](http://jsonlines.org/) format in which each line represents one sample. The sentences are specified using the `source` tag and the label can be specified using the `label` tag. Both `source` and `label` tags should be provided under the `AttributeNames` parameter value as specified in the request.

```
{"source":"linux ready for prime time , intel says , despite all the linux hype", "label":1}
{"source":"bowled by the slower one again , kolkata , november 14 the past caught up with sourav ganguly", "label":2}
```

Multi-label training is also supported by specifying a JSON array of labels.

```
{"source":"linux ready for prime time , intel says , despite all the linux hype", "label": [1, 3]}
{"source":"bowled by the slower one again , kolkata , november 14 the past caught up with sourav ganguly", "label": [2, 4, 5]}
```

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
