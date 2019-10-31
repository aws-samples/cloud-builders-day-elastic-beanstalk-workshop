---
title: "Developing and Deploying Applications"
date: 2019-04-15T17:58:18+03:00
weight: 2
---


## Objectives

In this lab you will quickly develop, build, and deploy an applications on AWS. 





## Services Used in this Lab:

- *AWS CodeStar* enables you to quickly develop, build, and deploy applications on AWS. AWS CodeStar provides a unified user interface, enabling you to easily manage your software development activities in one place. 
   - With AWS CodeStar, you can set up your entire continuous delivery toolchain in minutes, allowing you to start releasing code faster. AWS CodeStar makes it easy for your whole team to work together securely, allowing you to easily manage access and add owners, contributors, and viewers to your projects. 
   - Each AWS CodeStar project comes with a project management dashboard, including an integrated issue tracking capability powered by Atlassian JIRA Software. With the AWS CodeStar project dashboard, you can easily track progress across your entire software development process, from your backlog of work items to teams’ recent code deployments.
   - There is no additional charge for using AWS CodeStar.
   - Templates: You can choose from a variety of AWS CodeStar templates for Amazon EC2, AWS Lambda, and AWS Elastic Beanstalk. 
   - Repo: You have the option to choose AWS CodeCommit or GitHub to use as your project’s source control. 
   - IDE: You also have the option to edit your source code using one of several options including AWS Cloud9, Microsoft Visual Studio, or Eclipse.
   - Languages: AWS CodeStar project templates include the code for getting started on supported programming languages including Java, JavaScript, PHP, Ruby, C#, and Python.

- *AWS Elastic Beanstalk* is an easy-to-use service for deploying and scaling web applications and services developed with Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker on familiar servers such as Apache, Nginx, Passenger, and IIS.
   - Upload your code and Elastic Beanstalk automatically handles the deployment, from capacity provisioning, load balancing, auto-scaling to application health monitoring. 
   - You retain full control over the AWS resources powering your application and can access the underlying resources at any time.
   - There is no additional charge for Elastic Beanstalk 

- *AWS CodeCommit* is a fully-managed source control service that hosts secure and scalable Git-based repositories. 

## Lab Prerequisites

None.

## Expected Costs

The EC2 used in the labs is free tier eligible.


## Lab Steps

 
1. Go to AWS Console and Open CodeStar. Filter our 'Elastic Beanstalk' services and select 'Python - Flask' template

    ![EB](/images/labwa/eb1codestar.png) 

1. Give your project a name such as `cbd-eb-flask` and select `CodeCommit` as repo

    ![EB](/images/labwa/eb1eb2.png) 

1. Review project details. Understand how different AWS services are positioned at different stages of software development life cycle (SDLC)

    ![EB](/images/labwa/eb1eb3.png) 

1. Select the keypair you created in the beginning of the workshop [Prerequisites](/overview/prerequisites/)

    ![EB](/images/labwa/eb1eb4kp.png) 

1. Pick the Cloud9 IDE (This will create another Cloud9 IDE)

    ![EB](/images/labwa/eb1eb5.png) 

1. You are done. Wait for the environment to be ready.

    ![EB](/images/labwa/eb1eb6.png) 

    ![EB](/images/labwa/eb1eb7.png) 

1. The source code repo commit triggers a build and deployment process. 

    ![EB](/images/labwa/Picture8.png) 

1. Explore CodeCommit service

    ![EB](/images/labwa/eb1eb9cc.png) 

1. Explore CodeBuild service.

    ![EB](/images/labwa/eb1eb92.png) 

1. CodeBuild uses S3 to store bundles

    ![EB](/images/labwa/eb1eb93.png) 

1. You can access logs from the build process

    ![EB](/images/labwa/eb1eb94.png) 

1. In the backend, CloudFormation service is used to create all the resources for your scalable web app.

    ![EB](/images/labwa/eb1eb95.png) 

1. Finally, your web application is ready. 

    ![EB](/images/labwa/eb1eb96.png) 

1. The DNS is also provisioned for the web app.

    ![EB](/images/labwa/eb1eb97w.png) 

1. Let's look at the Elastic Beanstalk (EB) configuration. Observe that this is just a single node, non-scalable web app. One can think of it as a dev environment. 

    ![EB](/images/labwa/eb1eb98.png) 

1. EB simplifies access to underlying services (EC2, ELB, CloudWatch etc.). Go to Logs section and download the recent log files without logging in the server.

    ![EB](/images/labwa/eb1eb99.png) 

    ![EB](/images/labwa/eb1eb991.png) 

1. Check the Health section and observe the health state of the instance. 

    ![EB](/images/labwa/eb1eb992.png) 

1. Check Monitoring section and get a grip on various performance parameters.  

    ![EB](/images/labwa/eb1eb993.png) 

1. Check the events occurred.

    ![EB](/images/labwa/eb1eb994.png) 


## Summary
Congratulations! You've created a full CI/CD pipeline and deployed your web application to cloud.


## Deleting Lab Resources

We will this environment in the next lab. Please continue to the next lab. 

Otherwise terminate your EB, Cloud9 and CodeStar project resources.



