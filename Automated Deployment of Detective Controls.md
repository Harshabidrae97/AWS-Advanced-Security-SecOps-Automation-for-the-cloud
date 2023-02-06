# Automated Deployment of Detective Controls

## Introduction

This hands-on lab will guide you through how to use AWS CloudFormation to automatically configure detective controls including AWS CloudTrail, AWS Config, Amazon GuardDuty, and AWS Security Hub. You will use the AWS Management Console and AWS CloudFormation to automate the configuration of each service. The skills you learn will help you secure your workloads in alignment with the AWS Well-Architected Framework .

AWS CloudTrail is a service that enables governance, compliance, operational auditing, and risk auditing of your AWS account. With CloudTrail, you can log, continuously monitor, and retain account activity related to actions across your AWS infrastructure. CloudTrail provides event history of your AWS account activity, including actions taken through the AWS Management Console, AWS SDKs, command line tools, and other AWS services.

AWS Config is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources. AWS Config continuously monitors and records your AWS resource configurations and allows you to automate the evaluation of recorded configurations against desired configurations.

Amazon GuardDuty is a threat detection service that continuously monitors for malicious or unauthorized behavior to help you protect your AWS accounts and workloads. It monitors for activity such as unusual API calls or potentially unauthorized deployments that indicate a possible account compromise. GuardDuty also detects potentially compromised instances or reconnaissance by attackers.

AWS Security Hub is a service that aggregates, organizes, and prioritizes your security alerts, or findings, from multiple AWS services, such as Amazon GuardDuty, Amazon Inspector, Amazon Macie, AWS Identity and Access Management (IAM) Access Analyzer, AWS Systems Manager, and AWS Firewall Manager, as well as from AWS Partner Network (APN) solutions. AWS Security Hub continuously monitors your environment using automated security checks based on the AWS best practices and industry standards that your organization follows.
Prerequisites

    An AWS account that you are able to use for testing.
    Permissions to create resources in CloudFormation, CloudTrail, GuardDuty, Config, S3, CloudWatch.
