# Formula Instance Monitor

This example project uses `Cloud Elements Serverless Framework Plugin` to fetch information about a formula instance, feed that to AWS CloudWatch, create a dashboard, and create and alarm.


## Cloud Elements Serverless Framework Plugin

Information about the package can be found at [Cloud Elements Serverless Framework](https://www.npmjs.com/package/serverless-cloud-elements-plugin)

## Configuring the service

- Clone this repo
- [Update serverless.yml](#updating-serverless.yml)
- [Set up your environment](#setting-up-your-environment)
- [AWS Permissions](#permissions)
- [Deploy](#deploy)
- [Observe](#observe)

### Updating serverless.yml

#### Dashboard Name

Without changing anything your dashboard with be named after your insance with `-dashboard` appended

If your formula instance id is `12314` the dashboard will be named `12314-dashboard`

To change the name of the dashboard follow the instructions below:

##### ***OPTIONAL***

The field to change can be found on line 37 of the yaml or at `resources.Resources.dashboard.Properties.DashboardName`

  ![dashboard name pic](https://cl.ly/38f187ab5dae/[186b9595a5b212b2040f791029d74ecb]_Image%202019-03-22%20at%209.07.42%20AM.png)

#### Add Email Address(es) to SNS Topic

To add email addresses to the notificaiton subscription at this point enter your email address(es) on line 100 or at `resources.Resources.alarmNotificationSNSTopic.Properties.Subscription.Endpoint`

If you don't want to add any email notifications at this moment remove the subscription (lines 99-101)

![subcription view](https://cl.ly/133e8685bd0e/Image%202019-04-04%20at%201.47.30%20PM.png)

### Setting up your environment

#### Configuring your AWS creds with serverless

You need to generate or use an AWS key and secret. There are several ways to set this up but a simple way is:

```$ serverless config credentials --provider aws --key <your key here> --secret <your secret here>```

#### Set your environment variables

There are 4 variables to set:

1. USER_TOKEN
2. ORG_TOKEN
3. BASE_URL
4. FORMULA_INSTANCE_ID
5. FORMULA_INSTANCE_NAME

An example of setting an env variable

```$ export BASE_URL="https://api.cloud-elements.com"```

Please note that it is the formula **instance** id and name and not the formula template id/name

### Permissions

While this is not the the minimal permissions set needed from AWS these are what our users are configured for and therefore if your user has these there should be no issue deploying `formula monitor`

#### Standard Policy

- AmazonSQSFullAccess
- AWSLambdaFullAccess
- AmazonS3FullAccess
- AmazonAPIGatewayInvokeFullAccess
- CloudWatchFullAccess
- AmazonDynamoDBFullAccess
- AmazonAPIGatewayAdministrator
- AmazonSNSFullAccess

#### Inline Policy

```
{
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iam:PutRolePolicy",
                "iam:DeleteRolePolicy",
                "iam:CreateRole",
                "iam:DeleteRole"
            ],
            "Resource": [
                "arn:aws:iam::*:role/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudformation:*"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

### Deploy

```
$ npm install
$ sls deploy
```

### Observe

Navigate to the dashboard in AWS CloudWatch!

![cloud watch dashboard ss](https://cl.ly/dac6a8046069/Image%202019-03-22%20at%2010.06.14%20AM.png)