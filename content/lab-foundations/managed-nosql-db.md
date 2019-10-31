---
title: "Create a NoSQL DB in minutes"
date: 2019-03-29T17:59:09+03:00
weigth: 4
---


# Objectives

Let's create a NoSQL DB without server to manage and that can scale to tens of thosands of read/write requests per second. Instead of using the AWS Management Console, you will use the AWS Command Line Interface (AWS CLI) to manage DynamoDB auto scaling. 

## Services Used in this Lab:

- *Amazon DynamoDB:* Amazon DynamoDB is a key-value and document database that delivers single-digit millisecond performance at any scale. 
   - It's a fully managed, multiregion, multimaster database with built-in security, backup and restore, and in-memory caching for internet-scale applications. 
   - DynamoDB can handle more than 10 trillion requests per day and support peaks of more than 20 million requests per second. 
   - Many of the world's fastest growing businesses such as Lyft, Airbnb, and Redfin as well as enterprises such as Samsung, Toyota, and Capital One depend on the scale and performance of DynamoDB to support their mission-critical workloads.
   - More than 100,000 AWS customers have chosen DynamoDB as their key-value and document database for mobile, web, gaming, ad tech, IoT, and other applications that need low-latency data access at any scale. Create a new table for your application and let DynamoDB handle the rest. https://aws.amazon.com/dynamodb/ 

## Lab Prerequisites

Make sure you've gone through the Workshop prerequisites (Cloud9 IDE, AWS CLI etc.). 

## Expected Costs


## Lab Steps

The lab is based on this tutorial: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/AutoScaling.CLI.html We'll follow the instructions below:

1. Create a DynamoDB Table: In this step, you use the AWS CLI to create TestTable. The primary key consists of pk (partition key) and sk (sort key). Both of these attributes are of type Number. The initial throughput settings are 5 read capacity units and 5 write capacity units.

    ```sh
    aws dynamodb create-table \
        --table-name TestTable \
        --attribute-definitions \
            AttributeName=pk,AttributeType=N \
            AttributeName=sk,AttributeType=N \
        --key-schema \
            AttributeName=pk,KeyType=HASH \
            AttributeName=sk,KeyType=RANGE \
        --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5 –-region eu-west-1
    ```

1. To check the status of the table, use the following command:

    ```sh
    aws dynamodb describe-table \
        --table-name TestTable \
        --query "Table.[TableName,TableStatus,ProvisionedThroughput]" –-region eu-west-1
    ```

1. Register a Scalable Target: You will now register the table's write capacity as a scalable target with Application Auto Scaling. This allows Application Auto Scaling to adjust the provisioned write capacity for TestTable, but only within the range of 5 to 10 capacity units.

    ```sh
    aws application-autoscaling register-scalable-target \
        --service-namespace dynamodb \
        --resource-id "table/TestTable" \
        --scalable-dimension "dynamodb:table:WriteCapacityUnits" \
        --min-capacity 5 \
        --max-capacity 10 –-region eu-west-1
    ```

1. To verify the registration, use the following command:

    ```sh
    aws application-autoscaling describe-scalable-targets \
    --service-namespace dynamodb \
    --resource-id "table/TestTable" --region eu-west-1
    ```


1. Step 3: Create a Scaling Policy: In this step, you create a scaling policy for TestTable. The policy defines the details under which Application Auto Scaling can adjust your table's provisioned throughput, and what actions to take when it does so. Create a file named `scaling-policy.json` with the following contents:

    ```
    {
        "PredefinedMetricSpecification": {
            "PredefinedMetricType": "DynamoDBWriteCapacityUtilization"
        },
        "ScaleOutCooldown": 60,
        "ScaleInCooldown": 60,
        "TargetValue": 50.0
    }          
    ```

1. Use the following AWS CLI command to create the policy:

    ```sh
    aws application-autoscaling put-scaling-policy     \
    --service-namespace dynamodb     \
    --resource-id "table/TestTable"     \
    --scalable-dimension "dynamodb:table:WriteCapacityUnits"     \
    --policy-name "MyScalingPolicy"     \
    --policy-type "TargetTrackingScaling"     \
    --target-tracking-scaling-policy-configuration file://dynamo-scaling-policy.json --region eu-west-1


1. Wiew more details about the scaling policy

    ```sh
    aws application-autoscaling describe-scaling-policies \
        --service-namespace dynamodb \
        --resource-id "table/TestTable" \
        --policy-name "MyScalingPolicy" --region eu-west-1
    ```

## Lab Steps (Load Test the DynamoDB Table)

Let's drive Write Traffic to TestTable. 

Now you can test your scaling policy by writing data to TestTable. To do this, you will run a Python program. 

1. Create a file named bulk-load-test-table.py with the following contents:

    ```
    import boto3
    dynamodb = boto3.resource('dynamodb')

    table = dynamodb.Table("TestTable")

    filler = "x" * 100000

    i = 0
    while (i < 10):
        j = 0
        while (j < 10):
            print (i, j)
            
            table.put_item(
                Item={
                    'pk':i,
                    'sk':j,
                    'filler':{"S":filler}
                }
            )
            j += 1
        i += 1
    ```            

1. Type the following command to run the program:

    ```
    python bulk-load-test-table.py
    ```

1. Open AWS Console console. Go to DynamoDB service -> Tables -> Select TestTable. Select Metrics. Open the `Write Capacity` Metric in the dashboard and wait a few minutes. The provisioned write capacity for TestTable is very low (5 write capacity units) compared to the traffic generated by the python script, so the program stalls occasionally due to write throttling. This is expected behavior.

    ![DDB1](/images/labdyndb/d1asg1.png) 

1. From the same Metrics tab, open the `Throttled Write Events` Metric in the dashboard. You shall see the throttled write events. This is expected behavior.  In a real life situation, if it were 3 AM at Sunday night, you'd have had to wake up the DB admin from bed. 

    ![DDB2](/images/labdyndb/d1asg2.png) 

1. Now check the `Put Latency` metric. You should be seeing values less tnan 10 msec. This is the power of DynamoDB, which can deliver a *single digit milisecond* latency DB performance.

    ![DDB3](/images/labdyndb/d1asg3.png) 

1. Open the `Capacity` tab as seen below and wait for autoscaling event to pop-up. This event would indicate that Application Auto Scaling has issued an UpdateTable request to DynamoDB.

    ![DDB5](/images/labdyndb/d1asg5event.png) 

1. If you've seen the autoscaling event in the previous step, type the following command to verify that DynamoDB increased the table's write capacity: 
   - Command:

    ```sh
    aws dynamodb describe-table \
        --table-name TestTable \
        --query "Table.[TableName,TableStatus,ProvisionedThroughput]" --region eu-west-1
  
    ```

   - Output you shall see:

    ![DDB6](/images/labdyndb/d1asg6bw.png) 

 
 1. So if you have Autoscaling configured, the DB admin can goto sleep without worrying about throttling. 
 

## [Reading Exercise] Understanding How DynamoDB Auto Scaling Works
 
The following diagram provides a high-level overview of how DynamoDB auto scaling manages throughput capacity for a table:

![DDB6](/images/labdyndb/db-ug-asg.png)

The following steps summarize the auto scaling process as shown in the previous diagram:

   1. You create an Application Auto Scaling policy for your DynamoDB table.

   1. DynamoDB publishes consumed capacity metrics to Amazon CloudWatch.

   1. If the table's consumed capacity exceeds your target utilization (or falls below the target) for a specific length of time, Amazon CloudWatch triggers an alarm. You can view the alarm on the AWS Management Console and receive notifications using Amazon Simple Notification Service (Amazon SNS).

   1. The CloudWatch alarm invokes Application Auto Scaling to evaluate your scaling policy.

   1. Application Auto Scaling issues an UpdateTable request to adjust your table's provisioned throughput.

   1. DynamoDB processes the UpdateTable request, dynamically increasing (or decreasing) the table's provisioned throughput capacity so that it approaches your target utilization.


# Summary

Many database workloads are cyclical in nature or are difficult to predict in advance. For example, consider a social networking app where most of the users are active during daytime hours. The database must be able to handle the daytime activity, but there's no need for the same levels of throughput at night. Another example might be a new mobile gaming app that is experiencing rapid adoption. If the game becomes too popular, it could exceed the available database resources, resulting in slow performance and unhappy customers. These kinds of workloads often require manual intervention to scale database resources up or down in response to varying usage levels.

DynamoDB auto scaling uses the AWS Application Auto Scaling service to dynamically adjust provisioned throughput capacity on your behalf, in response to actual traffic patterns. 

## Deleting Lab Resources

1. Delete the scaling policy for TestTable:

    ```sh
    aws application-autoscaling delete-scaling-policy \
        --service-namespace dynamodb \
        --resource-id "table/TestTable" \
        --scalable-dimension "dynamodb:table:WriteCapacityUnits" \
        --policy-name "MyScalingPolicy" –-region eu-west-1
    ```

1. Deregister the scalable target:

    ```sh
    aws application-autoscaling deregister-scalable-target \
        --service-namespace dynamodb \
        --resource-id "table/TestTable" \
        --scalable-dimension "dynamodb:table:WriteCapacityUnits" –-region eu-west-1
    ```


1. Delete the TestTable table:


    ```sh
    aws dynamodb delete-table --table-name TestTable –region eu-west-1
    ```





