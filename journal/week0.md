# Week 0 — Billing and Architecture


Frontend - JS using react 
Backend - Python using flask - API = ORM (Object Relational Mapping)

Requirements for Good Architecture: -
Verifiable
Monitorable
Traceable
Feasible Ex - Meets ISO Standards
99.9% uptime
User can do specific tasks

The C4 model for visualizing Software Architecture Context Containers Components Code
Created Conceptual diagram and Logical diagram

Created account and setup prerequisite for all
1) Github account - https://github.com/PoorviPatel
2) Gitpod  Account
3) Github codespace accunt
4) AWS Account - 163430994021
5) Momento account
6) Custom Domain name - poorvi.cyou
7) Lucid chart
8) Honeycomb.io
9) Rollbar account

Pricing Basics and Free Tier
AWS Bill Free tier usage -Free tier utilization and it’s better to check regularly that you are not using beyond free tier -Change billing preferences to receive invoice by email, receive free tier usage alerts and receive Billing alerts

Billing Alert (CloudWatch Alarm & Budget) -To Create Alarm Select Create Alarm - > Select Metric - > Billing - > Total Estimate Charge - > USD - > Select Metric - > Setup you budget

![image](https://user-images.githubusercontent.com/36946649/219297022-d32317f8-1ec0-48e5-8ed9-a67a9e295ddc.png)

 
Setup cloudwatch billing alert create new alert - 10 alarms are free in free tier -Setup Budget using New Budget feature -There are only 2 free budgets in free tier
 
 ![image](https://user-images.githubusercontent.com/36946649/219297156-5ee6f92d-b277-4809-bb0c-f2a677f18f75.png)


Cost allocation tags -Create and Manage Instances and cost allocated with one project using EC2 management console

Cost explorer It helps to check cost and usage report using date range and other different filters. Helps to generate the reports when needed

Calculate AWS estimates cost for service

AWS credit Redeem AWS credit

 ![image](https://user-images.githubusercontent.com/36946649/219297213-c06687e5-2351-4bdc-be2d-4553b46fcbdb.png)


Free forever vs free for 12 months Explore free for 12 months and always free options for free and the conditions in the tier type



	


**Crudder Conceptual Diagram**
https://lucid.app/lucidchart/53c1cf25-169e-47ce-bdd2-ab432786e02f/edit?viewport_loc=-146%2C64%2C2220%2C974%2C0_0&invitationId=inv_aef3bde5-3d03-48da-872a-6afadd78656f

![image](https://user-images.githubusercontent.com/36946649/219843044-2b005f9c-8176-48a9-915b-744a40b05428.png)


**Crudder Logical diagram**
https://lucid.app/lucidchart/eef66c76-5f96-4589-be9f-4e8d265a954f/edit?viewport_loc=-1692%2C-462%2C2220%2C974%2C0_0&invitationId=inv_836ecba8-5036-4dc2-afe1-3294eff17186

![Crudder Logical diagram](https://user-images.githubusercontent.com/36946649/219843114-46ec443b-d980-4ba2-8107-66ace5554531.png)


### AWS IAM

Login to AWS -> IAN -> Create User


Set Permission
- Create user group
- User group name Admin - > Click Administrator Access	
- checkbox Admin and Create the User
- Save the console password
- Add the Alias name
- Login to console and change your password
	
Create secret access key and download csv file to save the access key

Launch cloudshell on AWS
	- To check who an I
	
	```sh
	aws --cli -auto-prompt
	aws sts get-caller-identity
	```
Install AWS in gitpod and also add the code to gitpod.yml
	
	```yml
	curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
     	unzip awscliv2.zip
      	sudo ./aws/install
	```
### Set env vars

Set the credentials

```
export AWS_ACCESS_KEY_ID=""
export AWS_SECRET_ACCESS_KEY=""
export AWS_DEFAULT_REGION=us-east-1
```

To make Gitpod remember these credentials if we relaunch our workspaces

```
gp env AWS_ACCESS_KEY_ID=""
gp env AWS_SECRET_ACCESS_KEY=""
gp env AWS_DEFAULT_REGION=us-east-1
```
## Enable Billing 

We need to turn on Billing Alerts to recieve alerts...


- In your Root Account go to the Billing Page
- Under Billing Preferences Choose Receive Billing Alerts
- Save Preferences


## Creating a Billing Alarm

### Create SNS Topic

- We need an SNS topic before we create an alarm.
- The SNS topic is what will delivery us an alert when we get overbilled
- aws sns create-topic

We'll create a SNS Topic
```sh
aws sns create-topic --name billing-alarm
```
which will return a TopicARN

We'll create a subscription supply the TopicARN and our Email
```sh
aws sns subscribe \
    --topic-arn TopicARN \
    --protocol email \
    --notification-endpoint your@email.com
```

Check your email and confirm the subscription

#### Create Alarm

- aws cloudwatch put-metric-alarm
- Create an Alarm via AWS CLI
- We need to update the configuration json script with the TopicARN we generated earlier
- We are just a json file because --metrics is is required for expressions and so its easier to us a JSON file.

```sh
aws cloudwatch put-metric-alarm --cli-input-json file://aws/json/alarm_config.json
```

## Create an AWS Budget

Get your AWS Account ID
```sh
aws sts get-caller-identity --query Account --output text
```

- Supply your AWS Account ID
- Update the json files
- This is another case with AWS CLI its just much easier to json files due to lots of nested json

```sh
aws budgets create-budget \
    --account-id AccountID \
    --budget file://aws/json/budget.json \
    --notifications-with-subscribers file://aws/json/budget-notifications-with-subscribers.json
```
