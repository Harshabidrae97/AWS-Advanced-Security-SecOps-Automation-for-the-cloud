# Create a Data Bunker Account

## Introduction

In this lab we will create a secure data bunker. A data bunker is a secure account which will hold important security data in a secure location. Ensure that only members of your security team have access to this account. In this lab we will create a new security account, create a secure S3 bucket in that account and then turn on CloudTrail for our organisation to send these logs to the bucket in the secure data account. You may want to also think about what other data you need in there such as secure backups.

![Screenshot 2023-02-05 at 2 32 18 PM](https://user-images.githubusercontent.com/55474202/216843325-126404f4-c375-4ea9-9489-457e61ba66e0.png)


. Create a logging account from the organizations management account

Best practice is to have a separate logging account for your data bunker. This account should only be accessible by folks in your security group with a read only role. How you create this account will depend on your organization’s policies, the instructions below are guidance on how to do this. If you do not currently have a landing zone setup see the quest Quick Steps to Security Success for a more in-depth discussion.

    Login to the management account of your AWS Organization
    If you do not have an account within your organization to store security logs. Navigate to AWS Organizations and select Create Account. Include a cross account access role and note it’s name (default is OrganizationAccountAccessRole) - we will modify this later to remove unnecessary access
    (Optional) If your role does not have permission to assume any role you will also have to add an IAM policy. The AWS administrator policy has this by default, otherwise follow the steps in the AWS Organizations Documentation to grant permissions to access the role
    Consider applying best practices as a baseline such as lock away your AWS account root user access keys and using multi-factor authentication
    Navigate to Settings and take a note of your Organization ID

2. Create a key to encrypt CloudTrail logs

    Switch roles into the logging account for your organization
    Navigate to AWS Key Management Service (KMS)
    Press Create a key
    Select Symmetric and press Next
    Enter an Alias for your key, for example CloudTrailKey

2. Create the bucket for CloudTrail logs

    While still in the logging account for your organization
    Navigate to S3
    Press Create Bucket
    Enter a Bucket name for your bucket, type a unique DNS-compliant name for your new bucket. Follow these naming guidelines:

    The name must be unique across all existing bucket names in Amazon S3.
    The name must not contain uppercase characters.
    The name must start with a lowercase letter or number.
    The name must be between 3 and 63 characters long.

    Choose an AWS Region where you want the bucket to reside. Choose a Region close to you to minimize latency and costs, or to address regulatory requirements. Note that for this example we will accept the default settings and this bucket is secure by default. Consider enabling additional security options such as logging and encryption, the S3 documentation has additional information such as Protecting Data in Amazon S3 .
    Accept default value for Block all public access.
    Enable bucket versioning , to keep multiple versions of an object so you can recover an object if you unintentionally modify or delete it.
    Click Create bucket.
    Press the bucket we just create and navigate to the Properties tab
    (Strongly recommended unless tearing down immediately) Under Object Lock, enable compliance mode and set a retention period. The length of the retention period will depend on your organizational requirements. If you are enabling this just for baseline security start with 31 days to keep one month of logs. Note: You will be unable to delete files within this window or the bucket if objects still exist in it
    Under the Permissions tab, replace the Bucket Policy with the following, replacing [bucket] and [organization id]. Press Save

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AWSCloudTrailAclCheck20150319",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudtrail.amazonaws.com"
            },
            "Action": "s3:GetBucketAcl",
            "Resource": "arn:aws:s3:::[bucket]"
        },
        {
            "Sid": "AWSCloudTrailWrite20150319",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudtrail.amazonaws.com"
            },
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::[bucket]/AWSLogs/*",
            "Condition": {
                "StringEquals": {
                    "s3:x-amz-acl": "bucket-owner-full-control"
                }
            }
        },
        {
            "Sid": "AWSCloudTrailWrite20150319",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudtrail.amazonaws.com"
            },
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::[bucket]/AWSLogs/[organization id]/*",
            "Condition": {
                "StringEquals": {
                    "s3:x-amz-acl": "bucket-owner-full-control"
                }
            }
        }
    ]
}

    (Optional) Next we will add a life cycle policy to clean up old logs. Navigate to Management
    (Optional) Add a life cycle rule named Delete old logs, press Next
    (Optional) Add a transition rule for both the current and previous versions to move to Glacier after 32 days. Press Next
    (Optional) Select the current and previous versions and set them to delete after 365 days

3. Ensure cross account access is read-only

These instructions outline how to modify the cross account access created in step 1 is read-only. As with step 1, this will depend on how your organization’s policies. The key is that our security team are not able to modify data in our data bunker. Human access should only be in a break-glass emergency situation.

Note: Following these steps will prevent OrganizationAccountAccessRole from making further changes to this account. Ensure other services such as Amazon Guard Duty and AWS Security Hub are configured before proceeding. If further changes are needed you will have to reset the root credentials for the security account.

    Navigate to IAM and select Roles
    Select the organizations account access role for your organization: Note: the default is OrganizationAccountAccessRole
    Press Attach Policy and attach the AWS managed ReadOnlyAccess Policy
    Navigate back to the OrganizationAccountAccessRole and press the X to remove the AdministratorAccess policy

4. Turn on CloudTrail from the management account

    Switch back to the management account
    Navigate to CloudTrail
    Select Trails from the menu on the left
    Press Create Trail
    Enter a name for the trail such as OrganizationTrail
    Select Yes next to Apply trail to my organization
    Under Storage location, select No for Create new S3 bucket and enter the bucket name of the bucket created in step 2

Verification

    Switch back to the Security account
    Navigate to the S3 bucket previously created
