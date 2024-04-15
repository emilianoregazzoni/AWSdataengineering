# AWS data engineering project :cloud:
## __Small hands-on project using data engineer tools from AWS__

In this project I decided to use some keys services from AWS to do some data engineering jobs. I used S3, Athena, Glue and Redshift :blush:

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

## __Glue section__
As we know, Glue is the main ETL service in AWS. You can use it for extract, transform, load data and do any transformation you want. Also is a serverless service, which means you can use it without managing any infrastructure, which is really cool and easier for anyone.
![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/5e549d59-a4f8-4197-851d-315ccaf664c6)

In this project I just decided to create a new column, depending on a given column. If a country has more export index, in the new column it will says "Exporting", and if a country has more import index, it will says "Importing".
  I found this simple dataset in Kaggle: https://www.kaggle.com/datasets/samira1992/countries-intermediate-dataset. Thank you Samira Shemirani.


Then I needed to give my personal user and also to a Glue role, the privilege to full access Glue control. This is for my personal user but for also, if in the future I want to create a lambda function that executes glue, then I need to create the role y assign it.

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/a2c1f901-e168-436b-a484-010b8d619c0b)

### TIP
If you want to explore AWS services, tools and so I recommend you to select just one region to work more confortable. Maybe later you can get confused about which services are being used in which region, which  could results in problems. All regions offer the same services for this project, and some services are global like IAM. I've choosen Ohio in the East.




