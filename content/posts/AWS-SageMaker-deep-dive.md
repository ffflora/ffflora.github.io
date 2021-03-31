---
title: "AWS SageMaker Deep Dive"
date: 2021-02-06T17:02:04-08:00
draft: true
toc: false
images:
tags:
  - untagged
---

This post consist of the notes that are based on the series of AWS SageMaker videos provided by Amazon Web Services [Amazon SageMaker Technical Deep Dive Series](https://www.youtube.com/playlist?list=PLhr1KZpdzukcOr_6j_zmSrvYnLUtgqsZz).

# Fully-Managed Notebook Instances with Amazon SageMaker

Create your Notebook Instance:

1. Pick the right family: 
   - t - tiny < m,   c - computed optimized, p - GPU
2. Pick the right size:
   - From medium to very very large.
3. Pick the right version:
   - ml.t**3**.medium: 3 means the latest version in the T family 

There are ~200 example notebooks in SageMaker Notebook, definitely try them out.



# Built-in Machine Learning Algorithms with Amazon SageMaker 

** = Distributed Training*

*<> = incremental training*

| Type of Problem      | Algorithms                                                   |
| -------------------- | ------------------------------------------------------------ |
| Classification       | Linear Learner *<br />XGBoost<br />KNN<br />Fatorization Machines |
| Computer Vision      | Image Classification <><br/>Object Detection<br/>Semantic Segmentation |
| Topic Modeling       | LDA<br/>NTM                                                  |
| Working with Text    | Blazing Text<br/>- Supervised<br/>- Unsupervised *           |
| Recommendation       | Fatorization Machines * (+ KNN)                              |
| Forecasting          | DeepAR *                                                     |
| Anomaly Detection    | Random Cut Forests *<br />IP Insights *                      |
| Clustering           | KMeans *<br />KNN                                            |
| Sequence Translation | Seq2Seq *                                                    |
| Regression           | Linear Learner<br />XGBoost<br />KNN                         |
| Feature Reduction    | PCA <br />Object2Vec                                         |





## 



## 



## 

