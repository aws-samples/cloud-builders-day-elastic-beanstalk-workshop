---
title: "Deploying a Scalable Web Application to Production"
date: 2019-04-15T17:58:18+03:00
weight: 4
---


## Objectives

In this lab you will quickly replicate your development environment you created in the previous lab and deploy to production with scalability enabled. 


## Services Used in this Lab:

- *AWS Elastic Beanstalk* is an easy-to-use service for deploying and scaling web applications and services developed with Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker on familiar servers such as Apache, Nginx, Passenger, and IIS.
   - Upload your code and Elastic Beanstalk automatically handles the deployment, from capacity provisioning, load balancing, auto-scaling to application health monitoring. 
   - You retain full control over the AWS resources powering your application and can access the underlying resources at any time.
   - There is no additional charge for Elastic Beanstalk 

- An open source tool `stress-ng` will be used for loading the instance. 

## Lab Prerequisites

You need to finish the previous lab [Developing and Deploying Applications](/overview/ci-cd-eb/).

## Expected Costs

If you do the load testing steps and spawn additional EC2 instances, additional costs will occur.


## Lab Steps

 
1. Go to the Elastic Beanstalk (EB) console and open the application you deployed in the previous lab. Open the environment as below. Select Actions -> "Clone Environment" from the menu. Add the suffic "-prod" to the previous environment name (e.g. cdb-eb-app to cdb-eb-app-prod)

    ![EB](/images/labwa/eb2prod0.png) 

1. Wait a few minutes until the new 'prod' environment is up and running. (Color will be changed from gray to green)

    ![EB](/images/labwa/eb2prod1.png) 

1. Go to the new prod environment. Select 'Configuration' from the left menu and do the following modifications:
1. In the 'Configuration' section check each section & try to understand options with the instructor.
1. Also check especially the `Rolling Updates & deployments section`. What are the deployment options? (Hing: All at once, blue/green, rolling ... )
1. Modify the Capacity Section as follows (when finished, press Continue):

        - Auto Scaling Group: Change `Single Instance` to `Load Balanced`
        - Instances Min / Max : `Min: 1 - Max: 2`
        - Select Triggers: 
            - Metric: `CPU Utilization`
            - Statistic: `Average`
            - Unit: `Percent`
            - Period: 2
            - Break duration: 2 
            - Upper threshold: 70
            - Scale up increment: +1
            - Lower threshold: 10
            - Scale up increment: -1

    ![EB](/images/labwa/eb2prod21.png) 

    ![EB](/images/labwa/eb2prod22.png) 

1. Confirm the warning about replacing current instance. Remember, this is a stateless web app, we don't have any special code or config on the EC2 instance. (Search for the Pets vs Cattles analogy for Cloud Applications)

    ![EB](/images/labwa/eb2prod3.png) 

1. After a few minutes, check your scalable web application. Try to identify the LB (load balancer), ASG (Auto Scaling Group) settings in the console. (Hint: ASG is under EC2 console. ELB has its own console.) Find out the EC2 that is part of the LB pool. 

    ![EB](/images/labwa/eb2prod4.png) 

1. Now let's test the behaviour of this system under load. Login to the EC2 instance of your production environment. Hints:
    - You'll need to use ssh either from your laptop or one of the your Cloud9 IDE terminals.
    - You'll need the keypair that you entered while creating the dev environment. Remember, it was the keypair you created in the beginning of the workshop.
    - You'll need to make sure the EC2 instance's security group gives access from internet.
    - Take your time to find out the solution, and ask your instructor if you are stuck.

1. If you managed to ssh to the production EC2 instance in your EB environment, try installing following tool and running the command.

    - Tool overview: 
    > stress-ng
    > stress-ng will stress test a computer system in various selectable ways. It
    > was designed to exercise various physical subsystems of a computer as well as
    > the various operating system kernel interfaces. Stress-ng features:
        > * over 210 stress tests
        > * over 50 CPU specific stress tests that exercise floating point, integer, bit manipulation and control flow
        > * over 20 virtual memory stress tests
        >  * portable: builds on Linux, Solaris, ...

    - install on Amazon Linux AMI as follows: 

    ```sh
    sudo yum -y install stress-ng
    ```

    - This command will keep the CPU busy (100%) for 10 minutes, which shall give enough time for the EB environment to react. 

    ```sh
    stress-ng --matrix 0 -t 10m
    ```
1. After a few minutes, observe the system. You shall see the initial instance marked as degraded, a new instance spawned. After the stress tool is finished. You should also see the additional instance being terminated. 

    ![EB](/images/labwa/eb2prod5.png) 

    ![EB](/images/labwa/eb2prod6.png) 

    ![EB](/images/labwa/eb2prod7.png) 


## Summary

Congratulations! You've created a production grade environment that gets feedback from the underlying resource utilization, scales up and down matching demand.


## Deleting Lab Resources

Terminate your EB, Cloud9 and CodeStar project resources you created in the previous lab.



