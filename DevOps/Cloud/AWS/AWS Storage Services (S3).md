![Untitled 111.png](Untitled%20111.png)

# Block

Block storage is technology that controls data storage and storage devices. It takes any data, like a file or database entry, and divides it into blocks of equal sizes. The block storage system then stores the data block on underlying physical storage in a manner that is optimized for fast access and retrieval. Developers prefer block storage for applications that require efficient, fast, and reliable data access. Think of block storage as a more direct pipeline to the data. By contrast, file storage has an extra layer consisting of a file system (NFS, SMB) to process before accessing the data.

## EBS

**EBS** (Elastic Block Store) - it is a block-level storage service offered by AWS that provides highly available, low-latency block-level storage volumes for use with EC2 instances. EBS volumes are designed for use with a single EC2 instance, and they can be attached and detached as needed. 

![Untitled 1 37.png](Untitled%201%2037.png)

Amazon Elastic Block Store (EBS) is an easy to use, high-performance, block-storage service designed for use with Amazon Elastic Compute Cloud (EC2) for both throughput and transaction intensive workloads at any scale. A broad range of workloads, such as relational and non-relational databases, enterprise applications, containerized applications, big data analytics engines, file systems, and media workflows are widely deployed on Amazon EBS.

You can choose from six different volume types to balance optimal price and performance. You can achieve single-digit-millisecond latency for high-performance database workloads such as SAP HANA or gigabyte per second throughput for large, sequential workloads such as Hadoop. You can change volume types, tune performance, or increase volume size without disrupting your critical applications, so you have cost-effective storage when you need it.

Designed for mission-critical systems, EBS volumes are replicated within an Availability Zone (AZ) and can easily scale to petabytes of data. Also, you can use EBS Snapshots with automated lifecycle policies to back up your volumes in Amazon S3, while ensuring geographic protection of your data and business continuity.

## EFS

**EFS** (Elastic File System) - it is a fully-managed, highly scalable file storage service that provides a simple and scalable file system for use with EC2 instances. EFS is designed to support multiple EC2 instances simultaneously, making it ideal for use cases where multiple instances need to access a shared file system. More information

![Untitled 2 27.png](Untitled%202%2027.png)

## EC2 Instance

An [instance store](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html) provides temporary block-level storage for your instance. This storage is located on disks that are physically attached to the host computer. Instance store is ideal for temporary storage of information that changes frequently, such as buffers, caches, scratch data, and other temporary content, or for data that is replicated across a fleet of instances, such as a load-balanced pool of web servers.

An instance store consists of one or more instance store volumes exposed as block devices. The size of an instance store as well as the number of devices available varies by instance type

# File

File storage stores data in a hierarchical structure of files and folders. In network environments, file-based storage often uses [network-attached storage (NAS)](https://aws.amazon.com/what-is/nas/) technology. NAS allows users to access network storage data in similar ways to a local hard drive. File storage is user-friendly and allows users to manage file-sharing control.

## EFS

**EFS** is an AWS file sharing service that lets you manage file shares, like those used on traditional networks, and mount them on cloud or on-premises machines using the **NFSv4 protocol**.

It is a scalable, cloud-based file system for Linux-based applications and workloads that can be used in combination with AWS cloud services and on-premise resources.

For more information, see EFS module.

**FSx** -provides you with the native compatibility of third-party file systems with feature sets for workloads such as Windows-based storage, high-performance computing (HPC), machine learning, and electronic design automation (EDA). More information

Amazon FSx provides you with the native compatibility of third-party file systems with feature sets for workloads such as Windows-based storage, high-performance computing (HPC), machine learning, and electronic design automation (EDA).  
You don’t have to worry about managing file servers and storage, as Amazon FSx automates the time-consuming administration tasks such as hardware provisioning, software configuration, patching, and backups.  
Amazon FSx integrates the file systems with cloud-native AWS services, making them even more useful for a broader set of workloads.  

![Untitled 3 22.png](Untitled%203%2022.png)

# Object

[Object storage](https://aws.amazon.com/what-is-cloud-object-storage/) is a technology that stores and manages data in an unstructured format called objects. Each object is tagged with a unique identifier and contains metadata that describes the underlying content. For example, object storage for photos contains metadata regarding the photographer, resolution, format, and creation time. Developers use object storage to store unstructured data, such as text, video, and images.

## S3

**S3** (Simple Storage Service) - it is a highly scalable, durable, and secure object storage service offered by AWS. S3 allows users to store and retrieve any amount of data from anywhere on the web and provides industry-leading scalability, data availability, security, and performance. It can be used for a wide range of use cases, including data lakes, backup and archive, and content distribution. More information

This services could be categorized by [Block](https://aws.amazon.com/what-is/block-storage/), [File](https://aws.amazon.com/what-is/cloud-file-storage/) and [Object](https://aws.amazon.com/what-is/object-storage/) storage type.

**Amazon Simple Storage Service**, widely known as **Amazon S3**, is a highly scalable, fast, and durable solution for object-level storage of any data type. Unlike the operating systems we are all used to, Amazon S3 does not store files in a file system, instead it stores files as objects. Object Storage allows users to upload files, videos, and documents like you were to upload files, videos, and documents to popular cloud storage products like Dropbox and Google Drive. This makes Amazon S3 very flexible and platform agnostic.

![Untitled 4 17.png](Untitled%204%2017.png)

![Untitled 5 17.png](Untitled%205%2017.png)

![Untitled 6 16.png](Untitled%206%2016.png)

![Untitled 7 14.png](Untitled%207%2014.png)

## Comparasion

|   |   |   |   |   |
|---|---|---|---|---|
||S3|EBS|EFS|FSx|
|**Used for**|provides access to reliable, fast, and inexpensive data storage infrastructure. Examples: data lake (data analytics, Back up and restore critical data, archiving data, storage for cloud-native applications)|provides block level storage volumes for use with EC2 instances. Examples: high-performance and high-availability block storage for mission-critical applications, databases storage, Easily resize clusters for big data analytics engines, such as Hadoop and Spark|provides scalable file storage for use with Amazon EC2. Examples: Share code and other files in a secure, organized way, Persist and share data from your AWS containers and serverless applications, Simplify persistent storage for modern content management system (CMS) workloads|is similar to EFS but provides support for popular commercial and open-source file systems. Examples: please see EFS column|
|**Used with**|Wide used object store service in AWS. For instance: RDS, API Gateway, Step Functions, etc.|EC2|EC2 linux-based systems with support NFSv4.1 can be integrated in serverless solution using access point|EC2 instances which support specific commercial and open-source file systems|
|**Configuration**|- S3 Standard for general-purpose storage of frequently accessed data - S3 Standard_IA for long-lived, but less frequently accessed data - S3 Glacier for long-term archive|SSD-based volumes: - General Purpose Volumes gp2, gp3 - Provisioned IOPS Volume io1, io2, io2 Block Express HDD-based volumes: - Throughput Optimized HDD volumes st1 - Cold HDD Volumes sc1|- Standard access - One Zone Access - Infrequent Access|Depends from file system: Lustre, ONTAP, OpenZFS, Windows File System|
|**Pricing**|Pay for using|Pay for provisioned capacity|Pay for using|Pay for using|
|**Storage capacity**|Unlimited|Limited, depends from volume type (max 16TB or 64TB)|Unlimited|Depends from file system (max for wfs - 65,536 GiB)|
|**Durability**|99.999999999%|99.8 - 99.999% depends from volume type|99.999999999%|Depends from file system (unspecified, 99.99%)|
|**Availability**|Depends from class 99.5 - 99.99% Multi AZ|99.999% Single AZ|99.99% Multi AZ|Depends from file system Multi AZ|
|**Accessibility**|Public/Private|Private (from an EC2 instance)|Private: EC2, integration with AWS services using access point|Private|

![Untitled 8 14.png](Untitled%208%2014.png)

Copy file

```Java
aws s3 cp| file1.html s3://s3-storage-nikita-katyshev/file1.html --profile FullS3
```

Using `aws s3 cp` from the [AWS Command-Line Interface (CLI)](http://aws.amazon.com/cli/) will require the `--recursive`parameter to copy multiple files.

```Plain
aws s3 cp --recursive s3://myBucket/dir localdir
```

The `aws s3 sync` command will, by default, copy a whole directory. It will only copy new/modified files.

```Plain
aws s3 sync s3://mybucket/dir localdir
```

```Bash
aws s3 rm s3://mybucket/test2.txt
```

```Java
aws s3 cp s3://nikita-katyshev-bucket1/ ./project --recursive
```

List file versions

```Bash
aws s3api list-buckets
```

```Java
aws s3api list-object-versions --bucket s3-storage-nikita-katyshev-task2 --prefix file2.html --query "Versions[?IsLatest].[VersionId]" --profile FullS3
```

```Java
aws s3api list-object-versions --max-items 1 --bucket $1 --prefix $2 --query "Versions [?LastModified<=$3]{version: VersionId}" --profile readOnlyAccessS3Mac
```

```Java
aws s3api list-objects-v2 --bucket s3-storage-nikita-katyshev-task2 --query "Contents[].[Key]" --profile FullS3
```

```Java
aws s3api list-objects-v2 --bucket s3-storage-nikita-katyshev-task2 --profile FullS3 --output table
```

|   |   |   |
|---|---|---|
|s3|s3api|s3control|
|These commands are designed to make it easier to manage your S3 files using the CLI.|These commands are generated from JSON models, which directly model the APIs of the various AWS services. This allows the CLI to generate commands that are a near one-to-one mapping of the service’s API|These commands allow you to manage the Amazon S3 control plane.|
|`**aws s3 ls**`|`**aws s3api list-objects-v2 \**``**--bucket my-bucket**`|`**aws s3control list-jobs \**``**--account-id 123456789012**`|

  

## Your goals

- Understand what AWS Elastic Block Storage is used for, know it’s volume types
- Explain EBS encryption, multi-attach, migration, monitoring and snapshotting, lifecycle manager
- Understand AWS EFS and its storage classes
- Know AWS EFS/EFx features and use-cases
- Know what AWS S3 is, its tiers, what bucket and object are
- Explain S3 replication, website hosting, ACL, versioning
- Understand S3 Security

## Self check:

1. Between block, file, object, which AWS services are related to each type?
2. What's the difference between EBS and EC2 Instance store?
3. If you need to have a folder, that will be used across a number of instance, what service will you use?
4. How can you replicate an EBS volume?
5. You have a static pages you want to expose to the Internet, what will you use for that case?
6. What are buckets in S3?
7. What does S3 replication do? What is it for?
8. What are versions in S3? What does it mean to delete an object in S3 when versioning is enabled?
9. How is it possible to optimize cost of S3 resources?
10. How nested file hierarchies are represented in S3?
11. What does S3 store inside its objects?
12. Why S3 is better than a physically maintained file server?
13. How many ways to allow access to S3 bucket do you know?
14. Is it possible attach EBS volume to a few instances simultaneously?
15. What is the use scenario for each Amazon FSx file system type?

Difference between AWS s3, s3api, and s3control

[https://towardsaws.com/querying-data-in-s3-using-amazon-s3-select-fe0b94519e42](https://towardsaws.com/querying-data-in-s3-using-amazon-s3-select-fe0b94519e42)