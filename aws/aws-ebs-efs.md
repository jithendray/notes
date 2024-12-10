---
tags: aws
---
# EBS vs EFS

## EBS

### What’s an EBS Volume?

- EBS = Elastic Block Store
- network drive we can attach to our instances while they run
    - not a physical drive
    - uses the network to communicate the instance — there might be bit of latency
    - can be detached from an instance & attached to a different one
- persist data, even after termination of instance
- can only be mounted to one instance at a time (CCP level)
- “multi-attach” feature for some EBS (Associate level)
- bound to a specific availability zone
    - EBS created in us-east-1a cannot be attached to instance in us-east-ib
    - to move across a volume, we first need to snapshot it
- EBS == network USB stick
- EBS = have a provisioned capacity (size in GBs and IOPS)
    - can increase the capacity over time
    - get billed for all the provisioned capacity
- multiple EBS volumes can be attached to a single EC2 instance
- EBS volumes can be left unattached - can be attached on-demand
- by default → “delete on termination” is enabled for the root volume
- by default → “delete on termination” is not enabled for the non-root or new volumes

### EBS Snapshots

- make a backup of EBS volume at any point of time
- not necessary to detach a volume to do snapshot, but recommended
- EBS Snapshot Archive
    - move a snapshot to an “archive tier” that is 75% cheaper
    - takes within 24-72 hrs for restoring the archive
- Recycle Bin for EBS Snapshots
    - rules - for retaining deleted snapshots (retention period - 1 day to 1 year)
- Fast Snapshot Restore (FSR)
    - forces full intialization of snapshot to have no latency on tghe first use — costs lots of money

### EBS Volume Types

- **General Purpose SSD** - gp2 / gp3 (SSD)
    
    - general purpose SSD volume
    - balances price and perofrmance
    - cost-effective, low latency
    - system boot volumes, virtual desktops, dev and test environments
    - 1 GB - 16 TB
    - gp3:
        - baseline of 3000 IOPS and throughput of 125 mbps
        - IOPS increase upto 16000
        - throughput increase upto 1000 mbps independent of IOPS
    - gp2:
        - small gp2 volumes can burst upto 3000 IOPS
        - size of volume and IOPS are linked, max IOPS is 16000
        - 3 more IOPS per GB → max 5334 GB - max IOPS
- **Provisioned IOPS (PIOPS) SSD** - io1 / io2 (SSD)
    
    - critical business applications with sustained IOPS performance
    - or applications that need more than 16000 IOPS
    - great for database workloads (sensitive to storage performance and consistency)
    - high-perofrmance SSD volumes
    - low-latency or high-throughput workloads
    - 4 GB - 16 TB
    - max PIOPS: 64000 for nitro EC2 instances & 32000 for other
    - can increase PIOPS independently from storage size (both io1 and io2 - just like gp3)
    - io2 have more durability and more IOPS per GB (at the same price as io1)
    - **io2 Block Express** (4 GB - 64 TB)
        - sub-millisecond latency
        - max PIOPS: 256000 with IOPS:GB ratio of 1000:1
    - supports **EBS Multi-attach**
        - attach same EBS to multiple EC2 instances in the same AZ
        - only available to io1 / io2
        - each instance has full read & write access
        - use-case:
            - achieve higher application availability in clustered linux applications
            - applications must manage concurrent write operations
        - up to 16 EC2 instances at a time
        - must use a file system thats’s cluster aware (not XFS, EX4 etc…)
- **Hard Disk Drives (HDD)** - st1 / sc1
    
    - cannot be boot volume
    - 125 MB - 16 TB
    - Throughput Optimized HDD - st1:
        - low cost HDD
        - for frequently accesses, throughput-intensive workloads
        - data warehouses, big data, log processing
        - max throughput - 500 mbps
        - max IOPS - 500
    - Cold HDD - sc1:
        - lowest cost HDD
        - for less frequently accesses workloads
        - scenarios where lowest cost is important
        - max throughput - 250 mbps
        - max IOPS - 250
- NOTE: only gp2/gp3 and io1/io2 can be used as boot volumes
    

## EC2 Instance Store

- EBS - network drives - good but limited performance
- for high-performance hardware disk - use EC2 Instance Store
    - better I/O performance
- if EC2 is terminated → EC2 instance store will lose their storage → ephemeral storage
- good for buffer / cache / temporary content
- but not for long-term storage
- risk of data loss if hardware fails
- You can run a database on an EC2 instance that uses an Instance Store, but you'll have a problem that the data will be lost if the EC2 instance is stopped (it can be restarted without problems). One solution is that you can set up a replication mechanism on another EC2 instance with an Instance Store to have a standby copy. Another solution is to set up backup mechanisms for your data. It's all up to you how you want to set up your architecture to validate your requirements. In this use case, it's around IOPS, so we have to choose an EC2 Instance Store.

## EFS

### what’s EFS?

- EFS = Elastic File System
- managed NFS (network file system) - can be mounted on many EC2
- works with EC2 instances in multi-AZ
- highly available, scalable, expensive (3 x gp2), pay per use
- use cases - content management, web serving, wordpress, data sharing
- uses NFSv4.1 protocol
- uses security group to cobtrol accesss to EFS
- compatible with only Linux AMIs - not windows
- encryption at rest using KMS
- POSIX file system that has standard file API
- no need to plan capacity in advance - scale automatically
- pay per use for each GB

### EFS scale

- 1000s of concurrenty NFS clients
- 10 GB + per second throughput
- grow to petabyte-sclae network file system - automatically
- Performance mode (set at EFS creation time)
    - general purpose (default): latency-sensitive use cases (web servers, CMS etc)
    - Max I/O — higher latency, throughput, highly parallel (big data, media processing etc..)
- Throughput mode
    - Bursting (1TB = 50 mbps + burst of up to 100 mbps)
    - Provisioned: set throughput regardless of storage size - ex: 1 gbps for 1 TB storage

### EFS storage classes

- storage tiers (lifecycle management feature - move files after N days)
    - standard: for frequently accessed files
    - Infrequent access (EFS-IA): cost to retrieve files, lower price to store, enable EFS-IA with a lifecycle policy
- availability and durability
    - Standard / regional: multi-AZ, great for prod
    - One Zone: great for dev, backup enabled by default, compatible for IA (EFS OneZone-IA)
    - over 90% in cost savings

## EBS vs EFS

- EBS volumes
    - can be attached to only one instance at a time
    - locked at AZ level
    - gp2: IO increases if disk size increases
    - io1: can increase IO independently
    - migrate EBS across AZ →
        - take snapshot
        - restore snap to anither AZ
    - EBS backups use IO and we shoud not run them while our application is handling a lot of traffic
    - root EBS of instances get terminated by default if EC2 instance gets terminates (can disable this)
- EFS
    - mouned to 100s of instances across AZ
    - only linux AMIs (POSIX)
    - higher price than EBS
    - for cost savings → EFS-IA
    - EFS - pay for only size we use but EBS - pay for pre booked capacity