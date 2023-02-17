# Week 0 — Billing and Architecture

## Required Homework
### Watched Week 0 - Live Streamed Video

This is the live stream for weeko of the Free AWS Cloud BootCamp. Student should either watch it live or stream it later, take note and follow along in their varorious cloud services applications and websites. Make sure to have All prerequite cloud services account Created.

https://www.youtube.com/watch?v=SG8blanhAOg&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=12


### Watched Chirag's Week 0 - Spend Considerations
https://www.youtube.com/watch?v=OVw3RrlP-sI&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=13

The is the pre-reorded video of Free AWS Cloud BootCamp - AWS Spend Consideration, presented by Chirag
Chirag teach us about the AWS billing console and the required alerts to set up for the AWS Cloud Boot camp specifically the free-Tier

#### Requirement
You will need your AWS Free-Tier Account

#### TOPICS
- AWS Bill and Free Tier Usage
    - first login into your AWS Account, IAM user with Billing access is preferable but if not you can also use your root account
    - if you do decided to user IAM user, use must assigned billing access becasue by default IAM user do ot have billing access.
    - search for billing in the search field and click on the billing Dashboard
    - The Billing Dashboard has suammry and detail AWS services costs
        - ### Bills  
      - Click on Bill and the Ammount is going to be in USD as well as your local currency
      - Also pricing varied by region, so make sure you are in a zone you will be spinning your services
      - summary - included summary of the bill
      - Details - show detail of services even if they are terminated
       - ### Free Tier Utilization
      - Click on the free tier - this is where you can track the services you are using and the forecast.
      - You can create up to 10 alarms under the Free-Tier and more than 10,  will incur a cost.
     
- Billing Alert 
  - there are two methods to setup Billing alerts
  - ### Method 1
    - click on the billing preferences
      - check desire preferences, enter your email addr. and save the settings   
  
      -  click on manage billing within the billing preferences page
          - to set up the cloudwatch, you must be in the N. Virginbia region
          - click on alarm and then click on alarm again and click on create alarm, select  metric and then click on "billing" and then total estimated charges, chec USD and click on select metric
          - Give your metric a name,  check the rest of the configuration and preview and create you alert.
          -   
   - ### method 2 click on budget
     - create a budget ( upto 2 budget for free)
 
- Cost Explorer - provides useful tools to help you gather information related to your cost and usage, analyze your cost drivers and usage trends, take actions to budget your spending, identify cost anomalies, and purchase savings plans.
- This is where you can create report

- Calculate AWS estimates cost for services suc as ec2
    - to calculate an estimate of the ec2 or any aws services, search for service pricesing in google for example EC2
    - seaerch ec2 pricing,
    - go to ec2 ondemand pricing, select your region and your ec2 types: m6i
    -get the ondemad hourly cost, which at this ttime was $0.12 per hour
    - multiple $0.12 by 730 hours ( hours in a month) 
    - no check the finding again aws calculato: go to calculator.aws
    -create your scenarior and check the total in the bottom.




- Check AWS Credit( voucher)
  - to chec or reddem yopu voucher/credit for AWS, click on credit  in ASW billing page
  - click redeem credit
  - clic applicabale product to see the list of eligible services for the voucher
  
- Cost allocation tags - allow you to set a tag on a service such as ec2 using keyword and then track the cost associated with the tag in cost allocation tag

- Comparison between Free Forever Vs. Free for 12 months
  - search for AWS free-tier and explore the free tiers
    - 12 months free
    - Alway free
    - Trials

If you spinned up a service and forget to shutdown or terminate it, you can cont open up a support case



### Watched Ashish's Week 0 - Security Considerations
https://www.youtube.com/watch?v=4EMWBYVggQI&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=15

The is the pre-reorded video of Free AWS Cloud BootCamp - AWS Spend Consideration, presented by Ashish Rajan. 

Ashish covers CLOUD security which covers AWS ordanization and the importantce of cloud security, IAM role and best cloud security practices 
  - Organizational unit
  - AWS CLOUD Shell - 
  - AWS cloud trail - logs activity accross AWS
  - AWS user - 3 types of user
    - IAM Users - An IAM user is an identity with long-term credentials that is used to interact with AWS in an account.
      - IAM role - An IAM role is an identity you can create that has specific permissions with credentials that are valid for short durations. Roles can be assumed by entities that you trust.
       
      - IAM Policy  - A policy is an object in AWS that defines permissions.
    - System User
    - root user
  - AWS Organization SCP
  - AWS SCP Best practices - check out Hashis' REPO - hashishrajan/aws-scp-best-practice-policies
  - Top 5 AWS Best Security Practices - 
    - Data protection and Residency in accordance to security policy
    - Identity & Access Management w/ least privilege
    - Governance & compliance of Aws Service being used
        - Global vs Regional services
        - Compliant services
    - Shared responsibily of the Threat Detection
    - Incident response plans to include cloud   

### Recreate Conceptual Diagram in Lucid Charts or on a Napkin

![Cruddur conceptual design!](https://media.discordapp.net/attachments/1057351515905458279/1076029660296654898/IMG_6672.jpg?width=639&height=618)

https://lucid.app/lucidchart/392f3e2f-e1f3-43a5-b61e-bd6e2d465ad8/edit?view_items=qhZxTb6PKSQF&invitationId=inv_77ef0804-46dd-4c72-aa06-00c894c01ad8

### Recreate Logical Architectual Diagram in Lucid Charts

### Create an Admin User 

### Use CloudShell

### Generate AWS Credentials

### Installed AWS CLI

### Create a Billing Alarm

### Create a Budget












## Homework Challenges

### Destroy your root account credentials, 
#### Set MFA, 

#### IAM role

## Health Dashboard
### Use EventBridge to hookup Health Dashboard to SNS and send notification when there is a service health issue.

## Well Architected Tool

### Review all the questions of each pillars in the Well Architected Tool (No specialized lens)

## LucidChart
  ### Create an architectural diagram (to the best of your ability) the CI/CD logical pipeline in Lucid Charts

## Research the technical and service limits of specific services and how they could impact the technical path for technical flexibility. 

## Open a support ticket and request a service limit
