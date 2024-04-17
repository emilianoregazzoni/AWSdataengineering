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
The format of the dataset is poor, just a .CSV like this:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/21f8c1bd-a40d-439c-977c-e2ea25669ea9)

Then I needed to give my personal user and also to a Glue role, the privilege to full access Glue control. This is for my personal user but for also, if in the future I want to create a lambda function that executes glue, then I need to create the role y assign it.

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/a2c1f901-e168-436b-a484-010b8d619c0b)

### TIP
If you want to explore AWS services, tools and so I recommend you to select just one region to work more confortable. Maybe later you can get confused about which services are being used in which region, which  could results in problems. All regions offer the same services for this project, and some services are global like IAM. So S3, Glue and Redshift ran into the same region. I've choosen Ohio in the East.

After reading the AWS documentation I understand that Glue service has attached some important concepts, like Crawler. A Crawler is: "A program that connects to a data store (source or target), progresses through a prioritized list of classifiers to determine the schema for your data, and then creates metadata tables in the AWS Glue Data Catalog."
It helps you to process the data, understand the format it cames, and transform into a Glue table in AWS:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/e8cf331d-b41f-4878-9e90-f70673dbe604)

But first, before I create the table, I needed to create the database in Glue. Just specify a name and set the route from S3, where you previously stored the .CSV

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/ed7d84c6-f723-4f2a-aed3-6a56584022b0)


![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/974fba6b-419c-4b8f-a1b3-05b2ed392adc)

Now I needed to create the table, using the Crawler, so I created a new Crawler:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/3a02f74d-834f-448b-b52b-cfa9b9bd6e2d)

I needed to set where the information is stored, in my case, I already created a bucket before, so I've choosen S3 and pasted the route from S3 like this:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/2b15ce14-2ff0-492e-acee-18b5fc810bf1)


Then I needed to select the Glue role I created before to perform.

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/88048070-e3b2-4906-a6a1-c160d7de2b46)

Then you can select if you want to run the Crawler with some periodicity or manually like I did:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/80306e31-817c-4c21-b188-e8ff0014760f)

After this, you can run the Crawler:
![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/c545f8d0-3622-4166-a761-3862bbbea144)

And then I saw the structure of the table created:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/3982da1e-54ca-4534-898e-d05c4f3e0b4e)

I noticed the Crawler created also a "partition":

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/05842aae-10a9-41fe-9880-e18e17b880ad)

A partition is created by Glue when it detects multiple folders in a bucket, it determines the root of a table in the folder structure and which folders are partitions of a table. The name of the table is based on the Amazon S3 prefix or folder name. It's usefull when you need to store data from certain dates, and you want this information later, in a table format, but classified with the data of the upload.

As I created the following structure:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/8c7d541e-743d-4d2b-bf3c-c2ecf575bf30)

I got the following partition:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/82eca292-566e-460f-bcd1-ee61c9aa44f2)

## __Athena section__

Athena is one of the AWS which allows you to query data. Is a serverless one, and I think is maybe the easiest service I used:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/7711f66d-9179-46c1-8857-7e4b40753384)

As you can see in the image, Athena can query from S3 buckets, Glue tables or data catalog (what I did).

I just need to search for Athena, click it and choose the database created previously in Glue, then I was available to query the data:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/6b676937-f17a-4156-b195-d611b5dccd28)

And I could see the information with the dataload column created, with this usefull behaviour I know when each record was uploaded:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/96990cf8-b271-43c6-b91c-319fe39d8265)

At this point I ended the uploading process, but know I needed to create the job itself:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/69cf8cb2-8238-4e4e-9175-ed7cc47ad6f6)

I needed to specify the source of information, in this case the new table created and then the interface of Glue allows you to design ETL jobs in a graphic way:

I set the data catalog configuration:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/0280befe-d0e1-45b3-ad57-7c368ea3206f)

Now what I wanted is to do a very simple transformation, so that Glue realizes that if the value in exports is greater than imports, then it is an exporting country, otherwise it is an importing country.

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/6255ba9d-6c6c-42a7-83c2-9df0f5ac6593)

First I tried in Athena if the logic were ok:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/947108cd-5acc-4888-bb5e-d0f55e7927e6)

And I got the column in the correct form:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/ed409633-cf2f-464c-ac0a-8e20685c030b)

So what I needed is just add this code into the step transformation in Glue:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/f084ff78-3a41-4b58-987c-e08664127e50)


At this time I had all the information and transformations I wanted prepared, ready to travel into Redshift.

## __Redshift section__

As Amazon says: "Amazon Redshift is a fast, fully managed, petabyte-scale data warehouse service that makes it simple and cost-effective to efficiently analyze all your data using your existing business intelligence tools. It is optimized for datasets ranging from a few hundred gigabytes to a petabyte or more and costs less than $1,000 per terabyte per year, a tenth the cost of most traditional data warehousing solutions."

Redshift is the most powerfull data warehousing solution AWS provides today. Is a must know how to deal with it, so I decided to load a new data warehouse with the country information.

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/0a8875d2-507a-40be-97dd-6f3e6b204b2a)

So I jumped into Redshift:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/be9033ff-70d1-41d0-bac5-47456db11859)

There are 2 ways to work with Redshift, you can work in a serverless way so you don't need to care about cluster management tasks, because is a fully managed services. It's called Redshift Serverless, or you can decide to work with Amazon Redshift that requires manual provisioning and scaling of clusters, demanding more management and static costs.
When I created the data warehouse I need to choose the "Namespace", namespace is like the name of the database. 
You just need to set a name and password to connect to database.

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/50e3199e-82f2-4040-a635-7b92f77f6022)

Then I had my Redshift service turned on.

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/3fb94b61-ac02-4261-9945-2c3a0951dff5)

The final step of the job was the load to databaset, the idea is to choose "Drop and recreate" the table, because there is no table created. So I wanted to create the table for the first time:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/2ef1eee9-6bf6-4686-873d-26d87dcd23b6)

Finally the job was this:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/01e0e537-4c12-4674-bdbd-028504491035)

But when I ran the job, I had a problem to deal with. The job failed constantly, it said: 

`VPC S3 endpoint validation failed for SubnetId: subnet-xxx. VPC: xxx. Reason: Could not find S3 endpoint or NAT gateway for subnetId:`

After reading a lot and investigating I figured out that I needed to create a S3 endpoint:

https://repost.aws/knowledge-center/glue-s3-endpoint-validation-failed

This article was very helpfull:

https://docs.aws.amazon.com/glue/latest/dg/vpc-endpoints-s3.html

What is an endpoint? An interface endpoint facilitates the connection between resources within the VPC and AWS services without the need for an Internet Gateway or VPN connection. This helps improve the security and latency of communications between VPC resources and AWS services.
If I don't create the endpoint, Glue can't access to S3, so I created the endpoint:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/8bd06e2d-71ae-413f-91c4-dec6b9287a8a)

Then I needed to choose my main VPC:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/763484a9-ff1b-4b93-8467-3b9528334033)

Then you need add the route tables yourself and it can inherit the ones used by the VPC, like this:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/c1c91abc-0e5a-4706-9d65-0e5fa40a2d98)

Finally I needed to create a connection in the Glue interface for Redshift. The connection must have the VPC and subnets properly configurated:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/9021caa1-3545-4454-a010-eea67c239cec)

After that I was able to run the job:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/1234eaaa-21ee-47ed-8e12-974f882df185)

Then I checked in Redshift the situation... and yes! The table was created successfully:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/b1827b51-f10f-42fc-a007-8e63c17e6075)


Now I tried some easy queries just to check if everything were running ok:

For example, the top 10 countries with worst numbers about child mortality:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/a03c4276-fe05-4d12-8126-c2c949990d7c)

The top five countries with most exports:

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/9a6465f0-c908-405f-beaf-6eccb7ddff45)

The top 10 importing countries with highest life expectancy (using the new column created in Glue):

![image](https://github.com/emilianoregazzoni/AWSdataengineering/assets/20979227/8db720a3-23b6-4ad0-8d12-6a5bef205001)

## __Costs__

This hands-on costed me around 4 USD, the main service used was Glue and Redshift servlerss, but AWS gives me a 300 USD free credits to use on Redshift with no charge.

## __Conclusions__

This simple project allowed me to explore some key services for data engineering purposes in AWS. You can work with a lot of more information and do the transformations you want. Glue is a really powerfull tool, you can do wherever you want in a interactive way without any code. Also it allows you to connect data sources out of AWS like BigQuery.
Redshift is an easy service to use, you can create powerful data warehouses in a very friendly way. You need to know some SQL and you will be fine exploring data.
It was an amazing experience to any AWS certified. I strongly recommend to start doing this kind of hands-on after the exam to challenge your skills! :blush:

### Thank you

Big thanks to Johnny Chivers for your Glue tutorial on Youtube, also to the Cloud Quick Labs channel on Youtube. I used also the AWS documentation from these services.
These 3 sources were my main help to explore and understand the operation of these services.

