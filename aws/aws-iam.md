---
tags: aws
---
# IAM Notes

### AWS Global Infrastructure ([https://infrastructure.aws/](https://infrastructure.aws/))

- **AWS Regions**
    - all around the word
    - names → us-east-1, eu-west-3
    - region = cluster of data centers
    - most AWS services are region-scoped
    - How to choose an AWS Region?
        - **Compliance** with data governance and legal requirements →data never leaves a region without your explicit permission
        - **Proximity** to customers → reduced latency → deploy the application where most users are → otherwise there might be lag
        - **Available services** → new services and features → not available in all regions
        - **Pricing** → varies from region to region
- **AWS Availability Zones**
    - region - uaually 3, min 2, max 6 availability zones
    - sydney → ap-southeast-2
        - ap-southeast-2a
        - ap-southeast-2b
        - ap-southeast-2c
    - each AZ → one or more discrete data centers with redundant power, networking, and connectivity
    - each AZ → separate from each other → so that they’re isolated from disasters
    - all AZs are connected wih high bandwidth, ultra-low latency networking
- **AWS Data Centers**
- **AWS Edge Locaions / Points of Presence**
    - 216 points of presence → 205 edge locations and 11 regional caches → in 84 cities across 42 countries
    - content is delivered to end users with lower latency
- **Note:** AWS has both global and region-scoped services
    - Global
        - IAM
        - Route 53
        - Cloud Front
        - WAF (Web Application Firewall)
    - region-scoped
        - EC2
        - Elastic Beanstalk
        - Lambda
        - Rekognition

## AWS IAM

### IAM: Users & Groups

- IAM = Identity and Access Management, Global service
- **Root account** - created by default - shouldn’t be used or shared
- **Users** - people within the organization - can be grouped
- groups only contain users, not other groups
- users dont have to belong to have any group
- users can belong to multiple groups

### IAM: Permissions

- Users and groups - can be assigned JSON documents called policies
- policies - define permissions of the users
- least privilege principle - dont give more permissions than what a user needs

### IAM: Policies

- policies are inherited
    
- IAM Policy structure
    
```json
{
  "Version": "2012-10-17",
  "Id": "S3-Account-Permissions",
  "Statement": [
    {
      "Sid": "1",
      "Effect": "Allow",
      "Principal": {
        "AWS": [
          "arn:aws:iam:123456789045:root"
        ]
      },
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:s3:::testbucket/*"
      ]
    }
  ]
}
```
    
- **Policy structure**
    
    - **Version**: plicy language version, always include “2012-10-17”
    - **Id:** identifier for the policy (optional)
    - **Statement**: one or more individual statements (required)
        - **Sid**: identifier for the statement (optional)
        - **Effect**: Allow / Deny
        - **Principal**: account/user/role to which the policy is applied to
        - **Action**: list of actions - policy allows or denies
        - **Resource**: list of resources to which actions applied to
        - **Condition**: conditions for which the policy is in effect (optional)

### IAM: Password Policy

- strong passwords - higher security for the account
- passowrd policy
    - set a min password length
    - require specific character types
    - allow all IAM users to change their own passwords
    - password expiration - requires users to change their password after sometime
    - prevent password re-use

### IAM: Multi Factor Authentication

- to protect root account and IAM users
- MFA = password you know + security device you own
- if password is stolen / hacked - the account is not comprimised
- MFA devices
    - virtual MFA device - google authenticator (phone only) / authy (multi-device) - support for multiple tokens on a single device
    - Universal 2nd Factor (U2F) security key - ex: Yubikey (3rd party) - supports multiple root and IAM users using a single security key
    - hardware key fob MFA device
    - hardware key fob MFA device for AWS cloud gov (USA)

### IAM users accessing AWS

- 3 ways to access AWS
    - AWS management console (password + MFA)
    - AWS command line interface (CLI) - protected by access keys
    - AWS software development kit (SDK) - for code: protected by access keys
- users manage their own access keys
- access keys are secrets (dont share them)
- **AWS CLI**
    - enables to interact AWS using commands from command-line shell
    - direct access to public APIs of AWS services
    - develop scripts to manage the resources
    - open-source
- **AWS SDK**
    - language-specific APIs
    - access and manage AWS services programatically
    - embedded inside applications
    - supports
        - SDKs - JS, Python, PHP, Java, etc…
        - Mobile SDKs - Android, iOS, etc…
        - IoT device SDKs - embedded C, Arduino, etc…

### AWS CLI - setup

- To install
```bash
curl "<https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip>" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```
    
- To configure - `aws configure`    
- `aws iam list-users`
- **AWS CloudShell** - terminal inside cloud
    - any files created inside cloud shell environment - will be saved
    - files can also be downloaded / uploaded

### IAM: Roles for Services

- Role → same as IAM user → but not for people, instead assign permissions to AWS services with IAM roles
- common roles
    - EC2 instance roles
    - Lambda function roles
    - roles for cloud formation

### IAM: Security Tools

- **IAM credentials report** → account-level → lists all users and the status of their various credentials
- **IAM access advisor** → user-level → permissions granted to users & when the services are last used → revise the policies to keep the least privilege

### IAM: Best Practices

- dont use root account - except when AWS account is setup
- one physical user = one AWS user
- assign users to groups & assign permissions to groups
- create strong password policies
- enforce the use of MFA
- create & use roles for giving permissions to AWS services
- use access keys for programtic access
- audit permissions using IAM credentials report / access advisor
- never share IAM user details & access keys