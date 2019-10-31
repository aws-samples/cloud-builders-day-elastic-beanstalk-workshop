---
title: "Cleaning up Resources You've Created"
date: 2019-03-29T17:39:55+03:00
weight: 12
---

Congratulations, you have managed to complete all the labs! This was intensive, but now we need to make sure that we clean-up our cloud account. This should take approximately 5 minutes, and ensures on-going costs are not incurred. 

Below is a reminder if you haven't terminated the resources. Perform the steps below, in the order that they are written.


## ML Module
1. Open the Amazon SageMaker service
1. Open the Notebook tab and select your notebook. Terminate it.
1. In the Models tab, delete the model which you have created in your lab.
1. In the Endpoints configuration tab, delete configurations which are present. In the Endpoints tab, delete the endpoint.

## Foundation Services

1. Open the CodeStar service and delete your project.
1. Open Cloud9 and delete your environment
1. Open EB and delete the production environment you've created.
1. Open DynamoDB and delete the table you've created
1. Open RDS and delete the instance you've created.

## Preparing Cloud Environment.

1. Open the Cloud9 service and delete your environment.
1. Open the EC2 service and delete all remaining EC2 instances.
1. In the EC2 service, go to Keys tab and delete your key.
1. Open the IAM service and then the users tab - you should delete the ide_user user. Open the Roles tab and delete roles that you have created.




