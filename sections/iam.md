# IAM: Identity Access & Management

- [IAM: Identity Access & Management](#iam-identity-access--management)
  - [What Is IAM?](#what-is-iam)
    - [IAM: Users & Groups](#iam-users--groups)
    - [IAM: Permissions](#iam-permissions)
    - [IAM Policies Inheritance](#iam-policies-inheritance)
    - [IAM Policies Structure](#iam-policies-structure)
    - [IAM – Password Policy](#iam--password-policy)
    - [IAM Roles for Services](#iam-roles-for-services)
    - [IAM Security Tools](#iam-security-tools)
    - [IAM Guidelines & Best Practices](#iam-guidelines--best-practices)
    - [Shared Responsibility Model for IAM](#shared-responsibility-model-for-iam)
  - [Multi Factor Authentication - MFA](#multi-factor-authentication---mfa)
  - [MFA devices options in AWS](#mfa-devices-options-in-aws)
  - [How can users access AWS ?](#how-can-users-access-aws-)
  - [What’s the AWS CLI?](#whats-the-aws-cli)
  - [What’s the AWS SDK?](#whats-the-aws-sdk)
  - [IAM Section – Summary](#iam-section--summary)

## What Is IAM?

AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS resources. You use IAM to control who is authenticated (signed in) and authorized (has permissions) to use resources.

### IAM: Users & Groups

- IAM = Identity and Access Management, Global service
- **Root account** created by default, shouldn’t be used or shared
- **Users** are people within your organization, and can be grouped
- **Groups** only contain users, not other groups
- Users don’t have to belong to a group, and user can belong to multiple groups

### IAM: Permissions

- Users or Groups can be assigned JSON documents called policies
- These policies define the permissions of the users
- In AWS you apply the least privilege principle: don’t give more permissions than a user needs

### IAM Policies Inheritance

![IAM Policies Inheritance](../images/IAM_Policies_inheritance.png)

### IAM Policies Structure

- Consists of
  - Version: policy language version, always include “2012-10-17”
  - Id: an identifier for the policy (optional)
  - Statement: one or more individual statements (required)
- Statements consists of
  - Sid: an identifier for the statement (optional)
  - Effect: whether the statement allows or denies access (Allow, Deny)
  - Principal: account/user/role to which this policy applied to
  - Action: list of actions this policy allows or denies
  - Resource: list of resources to which the actions applied to
  - Condition: conditions for when this policy is in effect (optional)

Example:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:Describe*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "elasticloadbalancing:Describe*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudwatch:ListMetrics",
        "cloudwatch:GetMetricStatistics",
        "cloudwatch:Describe*"
      ],
      "Resource": "*"
    }
  ]
}
```

### IAM – Password Policy

- Strong passwords = higher security for your account
- In AWS, you can setup a password policy:
  - Set a minimum password length
  - Require specific character types:
    - including uppercase letters
    - lowercase letters
    - numbers
    - non-alphanumeric characters
- Allow all IAM users to change their own passwords
- Require users to change their password after some time (password expiration)
- Prevent password re-use

### IAM Roles for Services

- Some AWS service will need to perform actions on your behalf
- To do so, we will assign permissions to AWS services with IAM Roles
- Common roles:
  - EC2 Instance Roles
  - Lambda Function Roles
  - Roles for CloudFormation

### IAM Security Tools

- IAM Credentials Report (account-level)
- a report that lists all your account's users and the status of their various credentials
- IAM Access Advisor (user-level)
- Access advisor shows the service permissions granted to a user and when those services were last accessed.
- You can use this information to revise your policies.

### IAM Guidelines & Best Practices

- Don’t use the root account except for AWS account setup
- One physical user = One AWS user
- **Assign users to groups** and assign permissions to groups
- Create a **strong password policy**
- Use and enforce the use of **Multi Factor Authentication (MFA)**
- Create and use Roles for giving permissions to AWS services
- Use Access Keys for Programmatic Access (CLI / SDK)
- Audit permissions of your account with the IAM Credentials Report
- **Never share IAM users & Access Keys**

### Shared Responsibility Model for IAM

| AWS                                      | YOU                                                                                                                      |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Infrastructure (global network security) | Users, Groups, Roles, Policies management and monitoring                                                                 |
| Configuration and vulnerability analysis | Enable MFA on all accounts                                                                                               |
| Compliance validation                    | Rotate all your keys often, Use IAM tools to apply appropriate permissions, Analyze access patterns & review permissions |

## Multi Factor Authentication - MFA

- Users have access to your account and can possibly change configurations or delete resources in your AWS account
- You want to protect your Root Accounts and IAM users
- MFA = password you know + security device you own
- Main benefit of MFA: if a password is stolen or hacked, the account is not compromised

## MFA devices options in AWS

- Virtual MFA device (Support for multiple tokens on a single device.)
  - Google Authenticator (phone only)
  - Authy (multi-device)
- Universal 2nd Factor (U2F) Security Key (Support for multiple root and IAM users using a single security key)
  - YubiKey by Yubico (3rd party)
- Hardware Key Fob MFA Device
- Hardware Key Fob MFA Device for AWS GovCloud (US)

## How can users access AWS ?

- To access AWS, you have three options:
  - AWS Management Console (protected by password + MFA)
  - AWS Command Line Interface (CLI): protected by access keys
  - AWS Software Developer Kit (SDK) - for code: protected by access keys
- Access Keys are generated through the AWS Console
- Users manage their own access keys
- Access Keys are secret, just like a password. Don’t share them
- Access Key ID ~= username
- Secret Access Key ~= password
- To get an Access Key ID and Secret Access Key for your AWS account, follow these steps:
- 1.	Open the AWS console and make sure you are logged in with your root username and password.
  2.	In the navigation bar, click on your username and select My Security Credentials.
  3.	Click the Continue to Security Credentials button.
  4.	Expand the Access Keys (Access Key ID and Secret Access Key) option.
  5.	To generate new access keys, click the Create New Access Key button.
  6.	Click Show Access Key to have it displayed on the screen. You can download it to your machine as a file and open it whenever needed. To download it, just click the Download Key File button.
- For IAM users:
- 1. Open the IAM console1.
  2. In the sidebar, click on Users.
  3. If you have an existing user you would like to generate credentials for, click on the user’s name.
  4. Click on the Security Credentials tab.
  5. Scroll down to the Access keys section and click on Create access key
  6. Download the file with the Access key ID and Secret access key1. Note that the Secret Access Key can only be retrieved at the moment of creation.

Note: you can see the AWS secret access key only once immediately after creating so, in order to get a secret key, you will need to create a new one. 
If you do not write down the key or download the key file to your computer before you press “Close” or “Cancel” you will not be able to retrieve the AWS secret access key in future. Then you’ll have to delete the keys which you created and start to create new keys.
As a best practice, AWS recommends creating an IAM user rather than relying on root access keys. IAM access keys allow you to securely control access to AWS services and resources for your users.
Remember to manage your access keys securely. Do not provide your access keys to unauthorized parties, even to help find your account identifiers. By doing this, you might give someone permanent access to your account.

  ![image](https://github.com/albertpsi/AWS-Certified-Cloud-Practitioner-Notes/assets/71813460/2d00eeed-4f81-49dd-9112-e60782c3678f)


## What’s the AWS CLI?

- A tool that enables you to interact with AWS services using commands in your command-line shell
- Direct access to the public APIs of AWS services
- You can develop scripts to manage your resources
- It’s open-source <https://github.com/aws/aws-cli>
- Alternative to using AWS Management Console

## What’s the AWS SDK?

- AWS Software Development Kit (AWS SDK)
- Language-specific APIs (set of libraries)
- Enables you to access and manage AWS services programmatically
- Embedded within your application
- Supports
  - SDKs (JavaScript, Python, PHP, .NET, Ruby, Java, Go, Node.js, C++)
  - Mobile SDKs (Android, iOS, …)
  - IoT Device SDKs (Embedded C, Arduino, …)
- Example: AWS CLI is built on AWS SDK for Python

## IAM Section – Summary

- **Users:** mapped to a physical user, has a password for AWS Console
- **Groups:** contains users only
- **Policies:** JSON document that outlines permissions for users or groups
- **Roles:** for EC2 instances or AWS services
- **Security:** MFA + Password Policy
- **AWS CLI:** manage your AWS services using the command-line
- **AWS SDK:** manage your AWS services using a programming language
- **Access Keys:** access AWS using the CLI or SDK
- **Audit:** IAM Credential Reports & IAM Access Advisor

* * *

[<img align="center" src="../images/back-arrow.png" height="20" width="20"/> What is Cloud Computing?](./cloud_computing.md)&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;[<img align="center" src="../images/list.png" height="30" width="30"/> List](../README.md)&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;[EC2: Virtual Machines <img align="center" src="../images/forward-arrow.png" height="20" width="20"/>](./ec2.md)
