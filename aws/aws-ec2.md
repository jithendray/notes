---
tags: aws
---

# EC2 notes

### Basics

- EC2 = Elastic Compute Cloud = IaaS
- it mainly consists in the capability of:
    - Renting virtual machines (EC2)
    - Storing data on virtual drives (EBS)
    - distributing load across machines (ELB)
    - Scaling the services (ASG)
- EC2 configuration options
    - which OS ? - linux, windows, Mac
    - how much compute power and cores ? - CPU
    - how much RAM?
    - how much storage space?
        - Network-attached (EBS & EFS)
        - hardware (EC2 Instance store)
    - network card? - speed of the card, Public IP address
    - firewall rules - security group
    - bootstrap script (configure at first launch) → EC2 User data
- EC2 has
    - private IPv4 → access instance on internal AWS network
    - public IPv4 → access instance from WWW
- if the instance is stopped & started again:
    - public IPv4 may change
    - private IPv4 will not change

### EC2 User Data

- EC2 user data script - for bootstraping the instances
- bootstrapping → launching commands when a machine starts
- this script - only run once at the instance first start
- responsible for automating boot tasks :
    - installing updates
    - installing software
    - downloading common files from the internet
    - etc….
- EC2 user data script runs with the root user

### EC2 Instance Types

- **General Purpose -** balance between compute, memory and networking resources
- **Compute Optimized -** intensive tasks that require high performance
    - batch processing workloads
    - machine learning
    - high performance computing
    - dedicated gaming servers
- **Memory Optimized -** fast performance workloads that process large data sets in memory
    - high performance databases
    - in-memory databases optimized for BI
    - applications performing real-time processing for big unstructured data
- **Storage Optimized -** great for storage-intensive tasks that require high, sequential read and write access to large datasets lke datawarehouse applications
- **Accelereated Computing -** use hardware accelerators, or co-processors, to perform functions, such as floating point number calculations, graphics processing, or data pattern matching, more efficiently than is possible in software running on CPUs.
- pricing and config details of all EC2 instance - [https://instances.vantage.sh/](https://instances.vantage.sh/)

### Connecting to EC2 using SSH (from Linux & Mac & Windows 10 machine)

- open terminal
- go to the folder where .pem file is present
- unprotect the .pem file
- connect to the ec2 instance using the pem file through the public IP of the instance
- type the following command

```bash
chmod 0400 fileName.pem

ssh -i fileName.pem ec2-user@pubicIP
```

### EC2 Instance Connect

- select the EC2 instance and click on connect

### EC2 IAM roles

- never enter personal AWS details in aws configure in EC2 instance
- create a role with **IAMReadOnlyAccess** and attach it to the instance

### EC2 Instance Purchase Options

- On-Demand Instances → short workload, predictable pricing
- Reserved (1 & 3 years) → long workloads
- Convertible Reserved Instances → long workloads with flexible instances
- Savings plans (1 & 3 years) → commitment to an amount usage, long workloads
- Spot Instances → short workloads, very cheap, less reliable (can loose instances)
- Dedicated Hosts → book an entire physical server, control instance placement
- Dedicated Instances → no other customer will share your hardware

**On demand Instances**

- pay for what you use
- has the highest cost but no upfront payment
- no long-term commitment
- recommended for short-term and un-interrupted workloads

**Reserved Instances**

- upto 75% discount as compared to On-demand
- we reserve InstanceType, Region, Tenancy, OS
- period - 1 year or 3 years
- payment - No Upfront (+), Partial Upfront (++), All Upfront (+++)
- scope - Regional or Zonal
- recommended for steady-state usage applications (like databases)
- can buy and sell in the reserved instance market place

**Convertible Reserved Instances**

- can change EC2 instance type, family, OS, scope and tenancy
- upto 66% discount

**EC2 Savings Plan**

- get a discount based on long-term usage (upto 75% discount)
- commit to a usage like → 10 USD per hour for 1 or 3 years
- usage beyond savings plan → billed at On-Demand price
- locked to a specific instance family & AWS region
- flexible across - instance size, OS, tenancy (host, dedicated, default)

**Spot Instances**

- discount upto 90% compared to On-demand instances
- instance can lost at any point of time - if max price is less than the current spot price
- useful for workloads that are resilient to failure - batch jobs, data analysis, image processing, distributed workloads, workloads with flexible start or end
- not suited for critical jobs or databases

**Dedicated Hosts**

- a physical server with EC2 instance capacity fully dedicated to our use
- allows us to address compliance requirements and use our existing server-bound software licenses (per-socket, per-core software licenses)
- may share hardware with other instances in same account
- On-demand or Reserved
- most expensive option
- useful for software that have complicated license model (BYOL - Bring Your Own License)
- companies with strong regulatory or compliance needs

**Dedicated Instances**

- instances runs on hardware dedicated for us
- may share hardware with other instances
- no control over instance placement (can move hardware after start / server)

**Capacity Reservations**

- reserve on-demand instances capacity in a specific AZ for any duration
- always have access to EC2 capacity
- no blling discounts, no time commitment
- combine with regional reserved instances and savings plan to benefit from billing discounts
- charged at on-demand rate whether instance is running or not
- suitable for short-term, uninterrupted workloads that needs to be in a specific AZ

### Which Instance launch type is better for me?

- On demand - like a resort, whenever we like - we pay the full price
- Reserved - if we plan to stay for a long time - we get good discount
- Savings Plan - pay a certain amount per hour for certain period and stay in any room with any facilities
- Spot Instances - bid for the empty room and highest bidder keeps the room, can get kicked out anytime if someone is willing to pay more than what we did
- Dedicated Hosts - book an entire building of resort
- Capacity Reservation - book a room for a period with full price even if we dont stay in it
### AMI

- AMI = Amazon Machine Image
- AMI → customization of an EC2 instance
    - add our own software, configuration, OS, monitoring…
    - faster boot / config time → sine all softare is pre-packaged
- AMI are built for a specific region (and can be copied across regions)
- can launch EC2 instances from:
    - Public AMI: AWS provided
    - own AMI: make and maintain them ourselves
    - AWS Marketplace AMI: made by someone else(and potentially sells)
- **AMI Process:**
    - start an EC2 instance and customize it
    - stop the instance - for data integrity
    - build an AMI - this will also create EBS snapshots
    - Launch instances from other AMIs

### Security Groups

- network security in AWS
    
- control traffic is allowed into or out of AWS services (EC2, RDS, RedShift etc)
    
- contain only allow rules
    
- SGs can be reference by IP or by security group
    
- security groups acts as firewall on EC2 instance
    
- they regulate:
    
    - access to ports
    - authorized IP ranges - IPv4 and IPv6
    - control inbnound network → from other to the instance
    - control outbound network → from instance to the other
- can be attached to multiple instances
    
- locked down to a region / VPC combination
    
- lives outside EC2 - if the traffic is blocked - EC2 instance wont see it
    
- **good to maintain one separate security group for SSH access**
    
- if the application is not acccessible (**timeout**) - security group issue
    
- if application gives a “connection refused” error - its a application error
    
- all inbound traffic is blocked by default
    
- all outbound traffic is authorized by default
    

### Classic ports to know

- 22 = SSG (Secure shell) - log into Linux instance
- 21 = FTP (File transfer protocol) - upload files into a file share
- 22 = SFTP (Secure File transfer protocol) = uploading files using SSH
- 80 = HTTP - access unsecured websites
- 443 = HTTPS - access secured websites
- 3389 = RDP (Remote Desktop Protocol) - log into a windows instance