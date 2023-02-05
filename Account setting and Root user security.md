# Generate and Review the AWS Account credentials Report
  * Its good to get an idea of what you have configured already in your AWS account especially if you have had it for a while. You should audit your           security configuration in the following situations:

    On a periodic basis. You should perform the steps described here at regular intervals as a best practice for security.
    If there are changes in your organization, such as people leaving.
    If you have stopped using one or more individual AWS services. This is important for removing permissions that users in your account no longer need.
    If you’ve added or removed software in your accounts, such as applications on Amazon EC2 instances, AWS OpsWorks stacks, AWS CloudFormation templates,     etc.
    If you ever suspect that an unauthorized person might have accessed your account.
    
  * As you review your account’s security configuration, follow these guidelines:

    Be thorough. Look at all aspects of your security configuration, including those you might not use regularly.
    Don’t assume. If you are unfamiliar with some aspect of your security configuration (for example, the reasoning behind a particular policy or the           existence of a role), investigate the business need until you are satisfied.
    Keep things simple. To make auditing (and management) easier, use IAM groups, consistent naming schemes, and straightforward policies.
    
    # AWS security audit guidelines
    
    * You should periodically audit your security configuration to make sure it meets your current business needs. An audit gives you an opportunity to           remove unneeded IAM users, roles, groups, and policies, and to make sure that your users and software have only the permissions that are required.
    
  # Review your AWS account credentials

  * Take these steps when you audit your AWS account credentials:

  * If you're not using the root access keys for your account, you can remove them. We strongly recommend that you do not use root access keys for everyday     work with AWS, and that instead you use users with temporary credentials, such users in AWS IAM Identity Center (successor to AWS Single Sign-On).

  * If you do need to keep the access keys for your account, rotate them regularly.
  
 # Review your IAM users

## Take these steps when you audit your existing IAM users:

    List your users and then delete users that are inactive.

    Remove users from groups that they don't need to be a part of.

    Review the policies attached to the groups the user is in. See Tips for reviewing IAM policies.

    Delete security credentials that the user doesn't need or that might have been exposed. For example, an IAM user that is used for an application does       not need a password (which is necessary only to sign in to AWS websites). Similarly, if a user does not use access keys, there's no reason for the user     to have one. For more information, see Managing Passwords for IAM users and Managing Access Keys for IAM users in the IAM User Guide.

    You can generate and download a credential report that lists all IAM users in your account and the status of their various credentials, including           passwords, access keys, and MFA devices. For passwords and access keys, the credential report shows how recently the password or access key has been       used. Credentials that have not been used recently might be good candidates for removal. For more information, see Getting Credential Reports for your     AWS Account in the IAM User Guide.

    Rotate (change) user security credentials periodically, or immediately if you ever share them with an unauthorized person. For more information, see       Managing Passwords for IAM Users and Managing Access Keys for IAM users in the IAM User Guide.
    
   # Review your IAM groups

    Take these steps when you audit your IAM groups:

    List your groups and then delete groups that are unused.

    Review users in each group and remove users that don't belong.

    Review the policies attached to the group. See Tips for reviewing IAM policies.


# Review your IAM roles

Take these steps when you audit your IAM roles:

    List your roles and then delete roles that are unused.

    Review the role's trust policy. Make sure that you know who the principal is and that you understand why that account or user needs to be able to           assume the role.

    Review the access policy for the role to be sure that it grants suitable permissions to whoever assumes the role—see Tips for reviewing IAM policies.


# Review your IAM providers for SAML and OpenID Connect (OIDC)

If you have created an IAM entity for establishing trust with a SAML or OIDC identity provider, take these steps:

    Delete unused providers.

    Download and review the AWS metadata documents for each SAML provider and make sure the documents reflect your current business needs. Alternatively, get the latest metadata documents from the SAML IdPs that you want to establish trust with and update the provider in IAM.
    
    
# Monitor activity in your AWS account

Follow these guidelines for monitoring AWS activity:

    Turn on AWS CloudTrail

in each account and use it in each supported Region.

Periodically examine CloudTrail log files. (CloudTrail has a number of partners

who provide tools for reading and analyzing log files.)

Enable Amazon S3 bucket logging to monitor requests made to each bucket.

If you believe there has been unauthorized use of your account, pay particular attention to temporary credentials that have been issued. If temporary credentials have been issued that you don't recognize, disable their permissions.

Enable billing alerts in each account and set a cost threshold that lets you know if your charges exceed your normal usage.

# Tips for reviewing IAM policies

Policies are powerful and subtle, so it's important to study and understand the permissions that are granted by each policy. Use the following guidelines when reviewing policies:

    As a best practice, attach policies to groups instead of to individual users. If an individual user has a policy, make sure you understand why that user needs the policy.

    Make sure that IAM users, groups, and roles have only the permissions that they need.

    Use the IAM Policy Simulator to test policies that are attached to users or groups.

    Remember that a user's permissions are the result of all applicable policies—user policies, group policies, and resource-based policies (on Amazon S3 buckets, Amazon SQS queues, Amazon SNS topics, and AWS KMS keys). It's important to examine all the policies that apply to a user and to understand the complete set of permissions granted to an individual user.

    Be aware that allowing a user to create an IAM user, group, role, or policy and attach a policy to the principal entity is effectively granting that user all permissions to all resources in your account. That is, users who are allowed to create policies and attach them to a user, group, or role can grant themselves any permissions. In general, do not grant IAM permissions to users or roles whom you do not trust with full access to the resources in your account. The following list contains IAM permissions that you should review closely:

        iam:PutGroupPolicy

        iam:PutRolePolicy

        iam:PutUserPolicy

        iam:CreatePolicy

        iam:CreatePolicyVersion

        iam:AttachGroupPolicy

        iam:AttachRolePolicy

        iam:AttachUserPolicy

    Make sure policies don't grant permissions for services that you don't use. For example, if you use AWS managed policies, make sure the AWS managed policies that are in use in your account are for services that you actually use. To find out which AWS managed policies are in use in your account, use the IAM GetAccountAuthorizationDetails API (AWS CLI command: aws iam get-account-authorization-details).

    If the policy grants a user permission to launch an Amazon EC2 instance, it might also allow the iam:PassRole action, but if so it should explicitly list the roles that the user is allowed to pass to the Amazon EC2 instance.

    Closely examine any values for the Action or Resource element that include *. It's a best practice to grant Allow access to only the individual actions and resources that users need. However, the following are reasons that it might be suitable to use * in a policy:

        The policy is designed to grant administrative-level privileges.

        The wildcard character is used for a set of similar actions (for example, Describe*) as a convenience, and you are comfortable with the complete list of actions that are referenced in this way.

        The wildcard character is used to indicate a class of resources or a resource path (e.g., arn:aws:iam::account-id:users/division_abc/*), and you are comfortable granting access to all of the resources in that class or path.

        A service action does not support resource-level permissions, and the only choice for a resource is *.

    Examine policy names to make sure they reflect the policy's function. For example, although a policy might have a name that includes "read only," the policy might actually grant write or change permissions.


