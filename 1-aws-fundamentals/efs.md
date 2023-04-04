# EFS - Elastic File System

- It is a managed NFS (network file system). It can be mounted on many EC2 instances
- EFS works with EC2 instances from multiple AZs
- Highly available, scalable
- It is expansive compared to EBS (3x gp2). It is pay per use, we only pay for what we use
- A security group should be attached to an EFS instance
- Multiple EC2 instances can access the files from an EFS instance in the same time
- Use cases for EFS: content management, web serving, data sharing
- Uses NFSv4.1 protocol
- **EFS is only compatible with Linux based AMIs!**
- EFS has a POSIX file system (because of Linux), that has a standard file API
- The EFS file system scales automatically, no capacity planning is required
- Encryption at rest for EFS is using KMS

## Performance & Storage Classes

- EFS Scale:
    - EFS is designed for thousands of NFS clients for reading and writing content at the same time
    - It provides 10+ GB/s throughput
    - It can grow to petabyte of storage
- Performance modes (should be set at the time of creation)
    - **General purpose** (default): used for latency-sensitive uses cases (web server, CMS)
    - **Max I/O** - higher latency, higher throughput, recommended for highly parallel workflows (big data, media processing)
- Throughput Mode
    - **Bursting** - 1 TB = 50MiB/s + burst of up to 100MiB/s
    - **Provisioned** - set your throughput regardless of storage size, ex 1GiB/s for 1TB storage
    - **Elastic** - automatically scales throughput up or down based on your workloads
        * Up to 3Gibs for reads and 1 GiB/s for writes
- Storage Tiers
    - **Standard**: should be used for frequently accessed files
    - **Infrequent Access (EFS-IA)**: should be used for less frequently accessed files. Has lower price for storage, but imposes some additional costs if a file is accessed
- EFS provides a lifecycle management feature for the files (similar to S3)

- Availability and durability
    - Standard: Multi-AZ, great for prod
    - One Zone: One AZ, great for dev, backup enabled by default, compatible with IA (Infrequent Access) (EFS One-Zone-IA)
- Over 90% in cost-savings

## EBS vs EFS

### EBS
- EBS volumes...
    - one instance (except multi-attach io1/io2)
    - are locked at the AZ (Availability Zone) level
    - gp2 IO increases if the disk size increases
    - io1 can increase IO independantly
- To migrate an EBS volume accross AZ
    - take a snapshot
    - Restore the snapshot to another AZ
    - EBS backups use I/O and you shouldn't run them while your application is handling a lot of traffic
- Root EBS Volumes of instances get terminated by default if the EC2 instance gets terminated. (you can disable this behavior)

### EFS
-   Mounting 100s of instances accross an AZ
-   EFS share website files (WordPress)
-   Only for LInux (POSIX) systems

- EFS has a higher pricepoint than EBS
- Can leverage EFS-IA for cost savings

- Remember: EFS vs EBS vs Instance Store




