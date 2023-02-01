# Detective Controls

## AWS Config 
* Assess, audit, and evaluate configurations of your resources
* AWS Config continually assesses, audits, and evaluates the configurations and relationships of your resources on AWS, on premises, and on other clouds.
![Screenshot 2023-01-29 at 11 13 43 PM](https://user-images.githubusercontent.com/55474202/215392880-0c72fdfb-0d8a-426a-a881-deb813d9edf4.png)

# How it works
* when you configure the AWS Config to your changing resources,
    * It creates time snapshots of all configurations and keeps mainating a record of all the data in your account.
    * The data will be normalized, so we can do version controling, apply some rules.
    * Now we have the data and rules, we can trigger some notifications is non-compliant.
    * we can use Amazon APIs and some SDKs to auto remediate some of the non complaints.
    * And we ca manage history and snapshots.

## AWS Config Aggregation
* An aggregator is an AWS Config resource type that collects AWS Config configuration and compliance data from the following:

    * Multiple accounts and multiple regions.

    * Single account and multiple regions.

    * An organization in AWS Organizations and all the accounts in that organization which have AWS Config enabled.

* Use an aggregator to view the resource configuration and compliance data recorded in AWS Config. The following image displays how an aggregator collects AWS Config data from multiple accounts and regions.
![Screenshot 2023-01-30 at 10 27 53 AM](https://user-images.githubusercontent.com/55474202/215535168-454319a4-e4aa-4345-a1fe-64d64a9232ae.png)

## Automatically Enforcing S3 Bucket Encryption.
 * S3 has 3 diff encryption : KMS, Server Side, Own keys.
 * If the bucket is created without encrytion, we are going deploy a script in the cloudformation which will take care of the encryption.
 * First we have config a rule, then there will be![Uploading Screenshot 2023-02-01 at 12.02.39 PM.png…]()
 periodical trigger. This trigger is going to evalate our S3 buckets.
 * In this case, its going to check weather the objects in the bucket or bucket itself has been encrypted.
![Screenshot 2023-02-01 at 12 03 06 PM](https://user-images.githubusercontent.com/55474202/216125696-e6fc82d9-0d07-4489-8130-23ad82950a0e.png)
* * Configured and ensured compliance for s3 buckets - automatically server side encryption using cloud formation and config.
* if anybody is trying to circumvent your rules, config ensure that configuration change is captured and it will enforce the compilance rules on top of it.


## Automatically enforce 'NO public IPs for EC2 instances' Ploicy
* Lets imagine an organization which says all of its insatnces should be behind a NAT instances or none of its EC2 instances should have a public IP address.

![Screenshot 2023-02-01 at 12 55 56 PM](https://user-images.githubusercontent.com/55474202/216137013-783adb5f-f64a-476f-8770-8c145c487ffe.png)

* what this rule will do is, it will check for the all the ec2 instances in the account which have public Ips and automattically STOP the instances.
* Even if somebody goes and attaches or launches a new instances with a public IP. This rule will enforce and turn off the instances.





