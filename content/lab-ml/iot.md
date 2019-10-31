---
title: "Serverless IoT Communication"
date: 2019-04-15T17:58:18+03:00
weight: 1
---

## Objectives

Create an IoT thing and start communicating with it.

## Services Used in this Lab:

- *AWS IoT Core* is a managed cloud service that lets connected devices easily and securely interact with cloud applications and other devices. AWS IoT Core can support billions of devices and trillions of messages, and can process and route those messages to AWS endpoints and to other devices reliably and securely. With AWS IoT Core, your applications can keep track of and communicate with all your devices, all the time, even when they aren’t connected.

## Concepts Used in the lab

- *Message Broker* – Acts as a front door for your device to the AWS Cloud. The message broker exposes publish-and-subscribe messaging capabilities available for use by devices through transport protocols like MQTT, HTTP, and WebSocket.

- *Rules Engine* – This is the point closest to the front door where your solution can make decisions about a device’s messages (for example, message filtering, routing messages to other services, and even a direct processing of messages in your solution). We will explore the facets of the rules engine in later posts.

- *Device Shadow* – Sometimes referred to as a thing shadow, this is a logical representation of a physical device’s reported state or desired future state. Although the concept is a bit abstract, this building block provides a huge benefit for customers who build web- or mobile-based applications that need to interact with real devices at any scale. In later posts, we will explore the ways in which the device shadow is used in these interactions.

- *AWS APIs and the AWS Management Console* – These are the mechanisms used to provision every AWS service. This post and all others in this series will include interaction with AWS through the APIs and AWS Management Console. In all Bites of IoT posts we assume you have an IAM user with developer-level permissions in an AWS account.

So the relationships between these building blocks is:
- AWS IoT has a message broker, rules engine, and device shadows.
- AWS IoT is a part of the AWS Cloud.

## Lab Prerequisites

None.

## Expected Costs

The IoT Core service is free tier eligible.


## Lab Steps
 
1. Make sure you are in the region you've been working so far (e.g. Ireland region).
1. Open the Amazon IoT Core service.
1. Click `Learn` from the bottom left corner of the page.
1. Select `See How AWS IoT Works` tutorial and watch the tutorial.

    ![EB](/images/labml/iot1iot1.png) 


1. From the same page, this time select `Connect to AWS IoT` and click `Get Started`
1. Select `Configuring a Device`.
1. Select `Linux` and `Python` 

    ![EB](/images/labml/iot1iot2.png) 

1. Register a thing with a name (e.g. `cbd_smart_device`)

    ![EB](/images/labml/iot1iot3.png) 

1. Based on the configuration a zip file will be generated (that is capable of running aws-iot-samples in github on a Linux platform via Pyhton. Download the file to your laptop
   
    ![EB](/images/labml/iot1iot5.png) 

1. Now upload it to your Cloud9 IDE (Open Cloud9, from the manu, select `File -> Upload a local file`)

    ![EB](/images/labml/iot1iot6.png) 


1. Go to the terminal tab of Cloud9 and create a folder called `iot-kit` and then move the zip file you downloaded to here: 

        ```sh
            cd ~/
            mkdir iot-kit
            mv <path-to>/connect_dev_package.zip .
        ```

1. As described in the instructions in the IoT Core page, unzip it, check what's inside, make the shell script executable and start the program (The author had a permission issue and therefore used the sudo command. You may want to check running ithout sudo as well)

        ```sh
            unzip connect_dev_package.zip

            chmod a+z start.sh
            sudo ./start.sh
        ```

1. The program installs the iot-samples from aws-labs at github: https://github.com/aws/aws-iot-device-sdk-python , downloads the certificates for securely communicating with IoT Core over MQTT. 

    ![EB](/images/labml/iot1iot7.png) 

1. The default program publishes` to a topic `sdk/test/Python` a message like below:

    ```
        {"message": "Hello World!", "sequence": 228}
    ```

1. Now go to the AWS IoT console. Select `Test` from the bottom left menu. 
1. Select `subscribe to a topic` and enter `sdk/test/Python` in topic name. You should be seeing the messages received by the IoT Core.

    ![EB](/images/labml/iot1iot8.png) 



## Summary
Congratulations! You've created a secure, scalable and serverless IoT communication between a simulated device and IoT Core. You can now take real-time action, do analytics, machine learning on the data ingested from billions of devices.


## Deleting Lab Resources

Delete your IoT thing at the end of the lab.


