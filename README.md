# AWS data engineering project :cloud:
## __Small hands-on project about AWS data engineer tools and concepts__

In this project I decided to use some keys services from AWS to do some data engineering job. I used S3, Athena, Glue and Redshift :blush:

First of all, key concept I learned from AWS Certified Cloud Practitioner, never ever use root user, you always need to create a IAM user with the least privilege possible. So I created my own user:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/22b0c5af-e091-448d-811b-d716a233c86f)

## __S3 section__

Then I needed to give my personal user and also to a Glue role, the privilege to full access Glue control. This is for my personal user but for also, if in the future I want to create a lambda function that executes glue, then I need to create the role y assign it.

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/a2c1f901-e168-436b-a484-010b8d619c0b)
