# AWS data engineering project :cloud:
## __Small hands-on project about AWS data engineer tools and concepts__

In this project I decided to use some keys services from AWS to do some data engineering job. I used S3, Athena, Glue and Redshift :blush:

First of all, key concept I learned from AWS Certified Cloud Practitioner, never ever use root user, you always need to create a IAM user with the least privilege possible. So I created my own user:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/22b0c5af-e091-448d-811b-d716a233c86f)

## __S3 section__
S3 is one of the most important services from AWS. It allow users to store unlimited amount of information in a bucket. A bucket is a public resource in the cloud, it provides object-based storage, where data is stored inside S3 buckets in distinct units called objects instead of files. A bucket is a container of files, it belongs to S3 environment.

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/50dba1db-2cdf-4f38-9e5e-a99c23b0b607)

In a bucket you can store, almost wherever you want.

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/11f18758-3734-40a9-9c7b-6b76dcd7bf94)

I decided to create the following structure in my bucket:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/10bec00a-532f-4604-8aed-a9fe7f0efdbb)

I have one folder for temporary files created by glue, a folder for data, a folder for scripts and finally a folder for athena query results (required).

Then I needed to give my personal user and also to a Glue role, the privilege to full access Glue control. This is for my personal user but for also, if in the future I want to create a lambda function that executes glue, then I need to create the role y assign it.

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/a2c1f901-e168-436b-a484-010b8d619c0b)
