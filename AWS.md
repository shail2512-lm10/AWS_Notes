# *AWS Services* #


## **1. IAM**
- Users
- Groups
- Policies (Access Permission policies)
- Roles for AWS services (Ex. EC2)

## **2. EC2** (Elastic Cloud Compute) (Iac)

- Only belongs to availibility zone (AZ) in which it is created
- Security group setup 
    - SSH: 22 
    - HTTP: 80
    - SFTP: 22
    - FTP: 21
    - HTTPS: 443
    - RDP: 3389

- SSH into EC2
    - either using .pem file on local machine

    `ssh -i accesskeyfile.pem ec2-user@public_ip_of_ec2_instance`
    
    - directly using EC2 instance connect. never put access key inside ec2 terminal. always use Roles created for AWS services by IAM

    - Using AWS SSM - Sessions Manager (need to create IAM role for that)

**AMI** (Amazon Machine Images)
- It is a customization of an EC2 instance
- AMI are built for specific regions and can be copied across regions
- Can create our own images from instances and use that image for creating new instance

**EC2 Image Builder**
- Used to automate creation of Virtual Machines or Containers
- Can be used to automate the creation, maintain, validate and test EC2 AMIs

## **3. EC2 Instance Storage**

**EBS** (Elastic Block Storage)
- EBS volume only belongs to availibilty zones (AZ)
- can only be mounted to 1 EC2 at a time.
- Define whether to delete the volume when instance is terminated, during the creation of instance.
- Possible to create snapshot, then create a volume from it and use it on instance in different AZ. Snapshot is a replica of EBS which is not in sync.
- can also copy a snapshot to different Region for backup purpose
- can also Archive Snapshot by moving to different storage tier from Actions button
- Setup Retention Rule for deleted snapshot in Recycle Bin

**EC2 Instance Store**
- EBS are network drives - good but limited performance
- EC2 instance store - High performance Hardware disk
- Good for buffer/cache/scratch data/temporary content
- Better I/O performance
- But lose thier storage if they are stopped

**EFS** (Elastic File System)
- Managed NFS (network file system) that can be mounted on 100s of EC2
- Same storage is shared by all connected EC2 instances

**Amazon FSx**
- Launch 3rd party high-performance file system on AWS

## **4. ELB** (Elastic Load Balancing)
Three types of Load Balancing Available
1. Application Load Balancer (HTTP/S, gRPC protocols):  
    - Static DNS (URL)
    - Routing based on: path in url, hostname in url, query string, headers
    - great fit for microservices & conatiner based apps,
    - Target groups: EC2, ECS tasks, Lambda Func, Private IP
2. Network Load Balancer (TCP / UDP protocols): 
    - Static IP through Elastic IP
    - Less latency, handle millions of requests per sec
    - 1 static IP per AZ
    - Target groups: EC2, Private IP, ALB
3. Gateway Load Balancer (GENEVE protocol on IP packets):
    - Deploy, scale, manage a fleet of 3rd party network visual appliances in AWS
    - Intrusion Detection, Firewalls, etc...
    - Target groups: EC2, private IPs
- While creating LB create a group of target instances and select the instances for which you want to create LB.

## **5. ASG** (Auto Scaling Groups)
- First create launch template which contains, AMI, instance type, key-pair and security groups
- Choose instance launch options
- config Load Balancer
- config group size and scaling policies

**ASG Scaling Startegies**
1. Manual Scaling
2. Dynamic Scaling
    - Simple/Step Scaling
    - Target Tracking Scaling
    - Scheduled Scaling
3. Predictive Scaling

## **6. Amazon S3**

- Main Building block of AWS
- Allows to store objects (files) in buckets (directories)
- Buckets are defined at a Region Level
- Objects (files) have a Key
- Key is the FULL path:
    - s3://my-bucket/my_folder/another_ffolder/my_file.txt
    - Key = s3://my-bucket + Prefix + object(file) name

**Security** (Bucket Policy)
- User-based: IAM Policies
- Resource-based: Bucket Policies (Use Policy Generator to create it)

**Features**

1. **Static Website Hosting**: 
- Can be used to host static website

2. **Versioning**
- Versioning of the bucket can be turned on. Bucket name > Properties > Bucket Versioning > Edit > Enable
- Permanently Delete: Enable Show Version > Delete the file with Version ID
- Delete Marker: Disable Show Version > Delete the file. The file will be still available when show version is enabled.
    - To restore it: Permanently delete the delete marker by after enabling the show version.

3. **Replication**
- versioning must be enabled in order to replicate the bucket
- Replication Possible on CRR, SRR, Different AWS account
- Must give proper read,write IAM permissions to S3
- Bucket > Management > Replication Rule > Create Rule > 
config destination bucket > choose create new IAM role > Save

4. **S3 Storage Classes**
- Can move between classes manually or using Lifecycle Policies.
- Storage Classes
    - Standard - General Purpose
    - Standard - IA
    - One Zone - IA
    - Glacier Instant Retrieval
    - Glacier Flexible Retrieval
    - Glacier Deep Archive
    - Intelligent Tiering
- Manually: Object > Properties > Change The Storage Class
- Lifecycle Policy: Bucket > Management > Create Lifecycle Rule > Apply to all objects in the bucket > Config > Create Rule

5. **S3 Encryption**
- Server-Side Encryption (Always ON)
- Client-Side Encryption

6. **AWS Snow Family**
- Allows for Large Scale Data Migration
- Snowcone
- Snowball Edge
- Snowmobile

7. **AWS Storage Gateway**
- Hybrid Cloud
- Bridge between on-premise data and cloud data in S3
- Types
    1. File Gateway
    2. Volume Gateway
    3. Tape Gateway

8. **S3 - Access Points**
- Create S3 Access point policy to grant access to specific prefix which gives access to specific users (Ex. Finance, Sales, etc...)

9. **S3 object Lambda**
- use lambda funcs to change the object befor it is retreived by the caller application
- S3 bucket > S3 access point > Lamda Func > S3 object Lambda access point


## **7. Databases**

**1. RDS**
- Managed DB service for DB. Allows you to create databases in the cloud that are maanged by AWS.
    - Potgres, MySQL, MariaDB, Oracle, Microsoft SQL Server, IBM DB2, Aurora (AWS proprietary db)
- Can cretae Snapshot of DB
- Restore, Copy, Share Snapshots

    **RDS Deplyment**
    - Read Replicas: Can create upto 15
    - Multi-AZ: Failover incase of AZ outage
    - Multi-Region: Read Replicas

**2. ElastiCache**
- Managed Redis or Memcached
- Caches are in-memory databases with high-performance, low latency

**3. DynamoDB**
- NoSQL Database (key/value database)
- Scales to massive workloads, distributed "serverless" database
- Single digit milisecond - Low latency retrieval
- low cost and auto scaling capabilities
- Standard and IA table class
- Global Tables: make accessible with low latency in multiple-regions. active-active replication (read/write to any AWS region)
- 1 RCU = 1 strongly consistent read per second for item upto 4 KB size
- 1 RCU = 2 eventually consistant read per second for 4 KB size
- 1 WCU = 1 write per second for 1 KB size
- to setup rcu,wcu: dynamodb table > additional settings > edit capacity
- DynamoDB Streams: can setup trigger for lambda function when the records of the tables have some change
    - DynamoDB Table > Exports and Streams
- TTL (Time to live): add expire timestamp, items will be automatically deleted after this timestamp

**4. DynamoDB Accelerator - DAX**
- Fully managed in-memory cache for DynamoDB
- 10x performance: microseconds latency
- DAX is only for DynamoDB whereas ElastiCache can be used for other DBs
- DAX > Clusters > Node families (all, t type, r type) > config networks > config security > advacned settings > create

**5. Redshift**
- Fully managed data warehouse service
- Based on PostgreSQl, but its not used for OLTP (online transaction processing)
- its OLAP (online analytical processing)
- load data every hour, not second
- columnar storage of data instead of row based
- Import/Export data to Redshift:
    - COPY command: from S3, EMR, DynamoDB, remote hosts
    - UNLOAD: unload from a table into files in S3
    - Enhanced VPC routing
    - Auto-copy from S3
    - Auto replication from Aurora
    - Streaming Ingestion from Kinesis Data streams or MSK
- Redshift Lambda UDF: execute lambda funcs in SQL queries
- Redshift Federated Queries: Ties Redshift to RDS or Aurora. It also eliminates the need for ETL 

**6. Amazon EMR (Elastic MapReduce)**
- Helps creating Hadoop clusters (big data)
- clusters can be made of 100s of EC2 instances

**7. Amazon Athena**
- Serverles query service to perform analytics against S3 objects
- uses standard SQL language to query the files

**8. Amazon Quicksight**
- serverless BI service to create interactive dashboards

**9. DocumentDB**
- DocumentDB is the same for MongoDB (Aurora for MongoDB)
- NoSQL Database
- Fully managed by AWS

**10. Amazon Neptune**
- Fully managed Graph database

**11. Amazon Timestream**
- Fully managed time-series database

**12. Amazon QLDB**
- Quantum Ledger Database
- A ledger is a book recording financial transactions
- used to review history of all the changes made to your application data over time
- Immutable system, cryptographically verifiable
- it is a centralized database

**13. Amazon Managed Blockchain**
- It is decentralized
- managed service to join public blockchain networks or create your own

**14. AWS Glue**
- Managed ETL service
- fully serverless service
- Useful to prepare and transform data for analytics

**15. DMS - Database Migration Service**
- Quickly, securely migrate dbs
- Supports Homogeneous migrations as well as Heterogeneous migrations

## **8. Other Compute Services**

**1. ECS** (Elastic Container service)
- Launch docker containers on AWS
- You must provision & maintain the infrastructure (EC2)
- AWS takes care of starting/stopping containers
- Has integrations with Load Balancer

**2. Fargate**
- Launch Docker Containers on AWS
- Dont need to provision the infra (EC2)
- Serverless offering
- AWS runs the container for you based on CPU/RAM you need

**3. ECR** (Elastic Container Registery)
- Private Docker Registery on AWS
store your Docker images here so that it can be run by Fargate or ECS

**4. AWS Lambda**
- Virtual functions - no servers to manage
- limited by time - short executions
- Run on-demand
- has time limit: 15 minitues
- limited temporary disk space
- serverless

**5. Amazon API Gateway**
- building a severless API
- serverless and scalable
- Supports restful APIs and websocket APIs

**6. AWS Batch**
- Fully managed batch processing at any scale
- Batch jobs are defined as Docker Images and run on ECS
- NO time limit
- Rely on EBS/instance store for disk space
- Relies on EC2

## **9. Deployments & Managing Infra at Scale**

**1. AWS CloudFormation**
- Infrastructure as Code
- Cost Savings
- Productivity
- We can see all the resources and relation between components

**2. AWS CDK**
- Cloud Development Kit
- Define Infra in your programming Language
- Code is compiled into a CloudFormaation Template. (Json/Yaml)

**3. AWS Elastic BeanStalk**
- Its a developer centric view of deploying an application on AWS
- Platform As a Service
- 3 architecture models
    1. single instance deployment
    2. LB + ASG
    3. ASG only (great for non-web apps in prod. ex.workers)

**4. AWS CodeDeploy**
- deploys application automatically
- Works with EC2 instances as well as on-premise servers

**5. AWS CodeCommit**
- Same as GitHub

**6. AWS CodeBuild**
- Code building service in cloud
- compiles source code, run tests, produces packages that are ready to be deployed (ex. by CodeDeploy)

**7. AWS CodePipeline**
- Orchestration Layer
- code > build > test > provision > deploy
- Basis for CI/CD

**8. AWS CodeArtifact**
- artifact management for software development
- can retrieve dependencies straight from CodeArtifact

**9. AWS CodeCatalyst**
- quickway to get started to correctly setup codecommit, codepipeline, codebuild, codedeploy, etc...
- can edit the code in-cloud using "AWS Cloud9" IDE

## **10. AWS Global Infrastructure**

**1. Route 53**
- Managed DNS (Domain NAme System)
- DNS is collection of rules & records which helps clients understand how to reach a server through URLs
- most common records are:
    1. A (IPv4)
    2. AAAA (IPv6)
    3. CNAME: hostname to hostname
    4. Alias (ex. ELB, S3, RDS, etc...)
- Routing Policies
    1. Simple Routing Policy: No Health Checks
    2. Weighted Routing Policy
    3. Latency Routing Policy
    4. Failover Routing Policy

**2. AWS CloudFront**
- Content Delivery Network (CDN)
- improves read performance, user experience
