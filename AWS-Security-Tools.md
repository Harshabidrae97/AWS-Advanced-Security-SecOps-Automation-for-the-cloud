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

