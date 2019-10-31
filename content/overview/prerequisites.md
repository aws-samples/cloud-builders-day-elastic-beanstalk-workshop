---
title: "Prerequisites"
date: 2019-03-29T17:39:55+03:00
weight: 1
---

# Preparing Cloud Environment

Your first lab will focus on preparing your cloud environment and will take around 10 minutes. You need to complete this first lab, otherwise it might be harder for you to complete the following labs. 

Services that you will be using in this lab: 

## AWS Account
- *AWS Identity and Access Management (IAM)* - AWS Identity and Access Management (IAM) enables you to manage access to AWS services and resources securely. Using IAM, you can create and manage AWS users and groups, and use permissions that allow or deny their access to AWS resources. IAM is a feature of your AWS account offered at no additional charge. You will be charged only for use of other AWS services by your users. You can find more information here: https://aws.amazon.com/iam/ 

   - *AWS Account:* You need to use your own AWS Account/user for this builders' day. If you don’t have admin rights, please walk through the following preparations with your AWS administrator. 

   - *Security Warning:* For the sake of simplicity, some of the permissions in the examples below are a bit too permissive. The permissions shall either be removed for the user after the builders' day, or they shall be turned into more fine-grained permissions. It is also not recommended to do the immersion from your AWS production account/user. 

## AWS Cloud9 
- *AWS Cloud9* - Cloud9 was one of the first “Development as a Service” platforms that delivered an integrated development environment in the cloud. AWS Cloud9 is a cloud-based integrated development environment (IDE) that lets you write, run, and debug your code with just a browser. It includes a code editor, debugger, and terminal. Cloud9 comes prepackaged with essential tools for popular programming languages, including JavaScript, Python, PHP, and more, so you don’t need to install files or configure your development machine to start new projects.  Features like collaborative coding, integration with Github and BitBucket, and easier deployment to PaaS and IaaS targets made it popular among developers. Founded in 2010 (before AWS acqurisiiton), Cloud9 supported some 40 different programming languages and lets remote teams work together to develop and edit code  and then test that code across some 300 different combinations of browsers and operating systems.  https://aws.amazon.com/cloud9/ 


- *A AWS Regions:* You can run the lab in any region. The examples will assume *EU Ireland* region.


## AWS Command Line Interface (CLI) 
The AWS Command Line Interface (CLI) is a unified tool to manage your AWS services. With just one tool to download and configure, you can control multiple AWS services from the command line and automate them through scripts. You can find more information here: https://aws.amazon.com/cli/ 

Steps: 

1. Log in to AWS Console 
1. Make sure that you are in EU Ireland region. The Ireland region will be used for the duration of the workshop. 
1. Open the AWS Cloud9 service. 
1. Click on Create environment button to create your first cloud environment. 
1. In the first step of the wizard provide name of your environment – for example CloudEnv. Then select Next step. 
1. Do not change any of the Configuration settings: leave them as set by default. Select Next step 
   - Note: In Configuration settings you choose the size of server on which your environment will be hosted. For the purpose of this workshop, a t2.micro instance will be more than enough. We will also leave the cost-saving settings alone; by default the cost-saving settings will auto-hibernate an instance after 30 minutes of inactivity. 
1. On the Review page, review the Environment name and settings for the Cloud9 service and select Create Environment to start the creation of the environment. 
1. It will take approximately 2-3 minutes for AWS Cloud9 to complete the creation your environment. Open another AWS Console session in another or new browser tab. 
   - Note: In our Cloud9 environment we will be using AWS CLI and EB CLI – which is already installed there. We will want to configure it with a new user which we will create in next steps. 
1. On the AWS Console, go to Services and open the IAM service and then select the Users page. 
1. On the Users page, select Add User. 
1. You will be taken to the 1st step of the Add user wizard. This user will be used for accessing the AWS CLI in your Cloud9 environment. Provide a User name* for your user e.g. ide_user 
1. Select the Programmatic access option and then select Next: Permissions 
![Add User](/images/prep1au.png)
1. On the 2nd step of the wizrd select Attach existing policies directly and choose the AdministratorAccess policy. Then select Next: Review 
![Review](/images/prep1rev.png)
1. On the 3rd step of the wizard, select Create user and confirm the creation of the user. 
1. On the 4th step you will need to download the credentials for your newly created user. This is done using the Download .csv button 
![Add User](/images/prep1au2.png)
1. Now let’s return to Cloud9 environment.
1. In terminal window of Cloud9 write the command aws configure 18) You will need to provide 4 values: 
1. Values 1 and 2 are the Access Key ID and Secret Access Key. These are found in the Download .csv file. Value 3. Is the name of your default region: Therefore, in our case this is eu-west-1 for EU Ireland 4. Is the default output format. We will use: json 
![AK](/images/prep1acckey.png) 
1. If you see a popup window informing about updating credentials: choose the Cancel option on the first window, and then choose Permanently disable on the second window. 
1. Before we continue, we will first create two other resources in the AWS Console. Leave Cloud9 open, and in another browser tab return to the AWS Console and open the EC2 Service. 
1. On the left side menu of EC2 Services, find and select Key Pairs. 
1. Select the Create Key Pair button. 
1. Provide a key pair name: (we suggested CloudKeys) and then select the Create button. 
![KP](/images/prep1keyp.png) 
1. After selecting the Create button, You should see that a .pem file is downloaded to your computer. This file will be named CloudKeys.pem or whatever you entered for the key pair name in the last step. 
1. Now we will create Security Group. Security Groups behave like a native firewall on AWS. Select the Security Groups item on the left side menu of EC2 Services window. 
1. Select the Create Security Group button. 
1. Provide a Security group name (we suggest CloudySG) and then provide a similar Description. 
1. In the Inbound roles tab create two rules for SSH and HTTP traffic. Do this by using the Add Rule button, selecting the appropriate Type, and setting the Source for both rules as Anywhere. 
![SG](/images/prep1sg.png) 
*Note:* In a working environment it is not good practice to generally open an SSH port to the whole world using the Anywhere source. It is done in this workshop for simplicity. 

# Summary 
In this lab you managed to create and configure your cloud environment using AWS Cloud9. You also used AWS Command Line Interface (CLI). Congratulations! 




