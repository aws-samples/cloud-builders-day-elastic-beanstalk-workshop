---
title: "Hosting a Global Static Website without Servers"
date: 2019-03-29T17:57:38+03:00
weight: 1
---

## Objectives
Let’s look into an interesting use case and see how you can create static websites without any servers. You can launch instances using AWS EC2 service, or AWS LighSail service, and we leave it to you to launch VM instances in the cloud. Amazon S3 can also be used for this purpose.


## Services Used in this Lab:
- *Amazon Simple Storage Service (Amazon S3)* is an object storage service that offers industry-leading scalability, data availability, security, and performance. This means customers of all sizes and industries can use it to store and protect any amount of data for a range of use cases, such as websites, mobile applications, backup and restore, archive, enterprise applications, IoT devices, and big data analytics. Amazon S3 provides easy-to-use management features so you can organize your data and configure finely-tuned access controls to meet your specific business, organizational, and compliance requirements. Amazon S3 is designed for 99.999999999% (11 9's) of durability, and stores data for millions of applications for companies all around the world. https://aws.amazon.com/s3/ 

- *Amazon CloudFront* is a fast content delivery network (CDN) service that securely delivers data, videos, applications, and APIs to customers globally with low latency, high transfer speeds, all within a developer-friendly environment. CloudFront is integrated with AWS – both physical locations that are directly connected to the AWS global infrastructure, as well as other AWS services. CloudFront works seamlessly with services including AWS Shield for DDoS mitigation, Amazon S3, Elastic Load Balancing or Amazon EC2 as origins for your applications https://aws.amazon.com/cloudfront/ 


## Lab Prerequisites

None

## Expected Costs

S3 and CloudFront services used in this tutorial are free-tier eligible.

## Lab Steps

### Hosting a Static Website without Servers 

You can host a static website on Amazon Simple Storage Service (Amazon S3). On a static website, individual webpages include static content. They might also contain client-side scripts. For instance, this is website, the author of this lab created using S3 and CloudFront: http://hanapublications.com 

![HBA](/images/labfund/f1hana.png) 
 

Let us walk you through the steps of hosting a website on Amazon S3.

1. This lab is documented here: (We won’t include the instructions in this guide).

https://docs.aws.amazon.com/AmazonS3/latest/dev/HostingWebsiteOnS3Setup.html


### Connecting your Website to Global CDN

Amazon CloudFront is a web service that speeds up distribution of your static and dynamic web content, such as .html, .css, .js, and image files, to your users. CloudFront delivers your content through a worldwide network of data centers called edge locations. When a user requests content that you're serving with CloudFront, the user is routed to the edge location that provides the lowest latency (time delay), so that content is delivered with the best possible performance.
You can create or update a distribution by using the CloudFront console or programmatically. This topic is about working with distributions by using the console.

1. This lab is documented here: (We won’t include the instructions in this guide).

 - https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-creating-console.html
 - https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html

