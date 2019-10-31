---
title: "ML for Neural Networks based Handwriting Recognition"
date: 2019-04-15T17:58:18+03:00
weight: 2
---

## Objectives

You willork with neural networks to build a model that will be able to recognize hand written digits.

## Services Used in this Lab:

- *Amazon SageMaker: * Amazon SageMaker provides every developer and data scientist with the ability to build, train, and deploy machine learning models quickly. Amazon SageMaker is a fully-managed service that covers the entire machine learning workflow to label and prepare your data, choose an algorithm, train the algorithm, tune and optimize it for deployment, make predictions, and take action. Your models get to production faster with much less effort and lower cost.  https://aws.amazon.com/sagemaker/
   - BUILD
        - Collect & prepare training data
        - Data labeling & pre-built notebooks for common problems
        - Choose & optimize your ML algorithm
        - Model & algorithm marketplace & built-in, high-performance algorithms
    - TRAIN
        - Setup & manage environments for training
        - One-click training on the highest performing infrastructure
        - Train & tune model
        - Train once, run anywhere & model optimization
    - DEPLOY
        - Deploy model in production
        - One-click deployment
        - Scale & manage the production environment
        - Fully managed with auto-scaling for 75% less


## Lab Prerequisites

None.

## Expected Costs

The SageMaker service is free tier eligible.


## Lab Steps

 
1. Make sure you are in the region you've been working so far (e.g. Ireland region).
1. Open the Amazon SageMaker service.
1. Select the Create notebook instance button.
1. Provide a name for your instance. For example: CloudyAI
1. In the IAM Role section, create a new IAM role for your notebook.
1. Select the Create notebook instance button. It will take about 15 minutes for your instances to be ready.
1. When your notebook is in the InService state, select the Open button.
1. Open the sample-notebooks / sagemaker-python-sdk / mxnet_gluon_mnist directory.
1. Open the mnist_with_gluon.ipynb file.

    ![EB](/images/labml/ml1ml1.png) 


1. Go through each step of the document by selecting Run button.
1. The longest step will be the learning/teaching model.

    ![EB](/images/labml/ml1ml2.png) 


1. When the model will be ready and deployed, you can test it in the browser by writing some digits.

    ![EB](/images/labml/ml1ml3.png) 

## Bonus Labs

Here are some other resources for you: 

1. Full day SageMaker Workshop: 
    - Agenda: https://s3.amazonaws.com/sagemaker-workshop/index.html
    - Lab Instructions: https://sagemaker-workshop.com/ You’ll start by creating a SageMaker notebook instance with the required permissions. You will then interact with SageMaker via sample Jupyter notebooks, the AWS CLI, the SageMaker console, or all three. During the workshop, you’ll explore various data sets, create model training jobs using SageMaker’s hosted training feature, and create endpoints to serve predictions from your models using SageMaker’s hosted endpoint feature.
1. SageMaker Intro Workshop (in Turkish): https://github.com/barisyasin/sagemaker-intro-tr 


## Summary
Congratulations! You've applied an image recognition ML algorithm using an open source framework (Gluon) without worrying about the infrastructure.

## Deleting Lab Resources

Terminate your SageMaker notebook instance.



