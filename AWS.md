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
1. Application Load Balancer (HTTP/S, gRPC protocols): Static DNS (URL)
2. Network Load Balancer (TCP / UDP protocols): Static IP through Elastic IP
3. Gateway Load Balancer (GENEVE protocol on IP packets): Intrusion Detection
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
- NoSQL Database
- Scales to massive workloads, distributed "serverless" database
- Single digit milisecond - Low latency retrieval
- low cost and auto scaling capabilities
- Standard and IA table class

**4. DynamoDB Accelerator - DAX**
- Fully managed in-memory cache for DynamoDB
- 10x performance: microseconds latency
- DAX is only for DynamoDB whereas ElastiCache can be used for other DBs