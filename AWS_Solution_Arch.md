Setup Elastic IP for EC2: Network & Security > Elastic IPs > Allocate Elastic IP > Actions > Associate Elastic IP > Resource type (Instance) > Associate

**Placement Groups**
- cluster: clusters instances into a low-latency group in a single AZ 
- spread: spreads instances across underlying hardware (max 7) on different AZ
- Partition: across many different partitions within an AZ. Scales to 100s of EC2 instances per group (Hadoop, Cassandra, Kafka)
- Network & Security > Placement Groups > Placement Strategy > Create
- While creating EC2 instance: Advanced details > Placement Group name > select 

**Elastic Network Interfaces** (ENI)
- Logical component in a VPC that represents a virtual network card
- ENI can have - primary private ipv4, 1 or more secondary ipv4, one elastic ipv4 per private ipv4, one public ipv4, one or more security groups, a MAC address
- can create ENI independently and attach them on the fly on EC2 instances for failover
- Bound to a specific AZ

**EBS Multi-Attach**
- upto 16 EC2 instances at a time
- must use a file system that is cluster-aware

**EBS encryption**
- Try to enable encryption at the EBS storage creation
- if you want to encrypt after the creation: snapshot the EBS > copy snapshot > enable encryption > create volume from snapshot

**EFS**
- uses NFSv4.1 protocol
- uses security group to control access to EFS
- compatible with only Linux based AMI
- encryption at rest using KMS
- EFS types
    - EFS scale
    - Performance mode: defualt, MAX I/O
    - Throughput mode: Bursting, Provisioned, Elastic
