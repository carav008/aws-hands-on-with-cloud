Account Basics

Create an IAM user for your personal use.
- login into console with root user 
- Navigate to IAM and create new user 

Set up MFA for your root user, turn off all root user API keys.
- click on top right corner -> my security credentials 
- Under Multi-factor authentication -> created new virtual MFA device 

Set up Billing Alerts for anything over a few dollars.

Configure the AWS CLI for your user using API credentials.
- install aws cli (and terraform for future use) 
- Verify 
```
$ aws version 
```
- Setup by credentials by running
``` 
$ aws configure
AWS Access Key ID []: ENTER_ACCESS_KEY 
AWS Secret Access Key []: ENTER_SECRET_ACCESS_KEY 
Default region name [us-east-1]: ENTER_REGION 
Default output format [json]: json
``` 

Checkpoint: You can use the AWS CLI to interrogate information about your AWS account.
```
$ aws help 
```

https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-commandstructure.html
```
# per docs above
$ aws <command> <subcommand> [options and parameters]
```

```
$ aws s3 ls
2020-11-19 14:55:14 projectomegabrian
2021-01-28 22:51:28 terraformstatebucket3655
```
