# Introduction
3 different types of storage:
- File based storage. Files have a name and are stored within a directory hierarchy.
  - Once number of files gets large, can get more expensive to find and access them
- Block storage. Files are split into chunks of equal size (blocks), and stored in "raw storage
  volumes" (I assume this means they don't have any filesystem already installed?).
  - Used by operating systems that can manage those volumes and use them as individual hard drives.
  - Used by databases, EC2 instances
  - RAID is when blocks are stored across multiple disks which are then accessible as a single
    logical volume
- Object storage
  - Basically a hashmap for data. Data is stored with a given name that is then presented to the
    object storage (which I assume does a hash of it - instructor talks of a "unique identifier"),
    which then makes it easier for the data to be retrieved (compared to file-based storage)

Services for each of the above types:
- File storage:
  - Amazon Elastic File System (EFS): scalable and elastic network file system.
  - Amazon FSx for Windows File Server: fully managed file storage for Windows Server
- Block Storage:
  - Amazon Elastic Block Store (EBS)
- Object storage:
  - Amazon S3.

Other storage services:
- AWS Backup: Centralised and automated data protection across AWS services
- AWS Storage Gateway: integrate on-premises applications with "virtually unlimited" AWS storage
  (S3, etc)

# Elastic File System (EFS)
Provides NFS for (Linux-based) EC2 instances.

Why?
- Might want multiple different instances (maybe running different workloads) to access shared data
- S3 has transfer costs and additional network latency, and also doesn't present itself as part of
  the instance's file system. NFS shines here.

EFS stores data redundantly across multiple AZs.

Has standard and infrequent storage classes, where infrequent is cheaper.
- Can have lifecycle policies to move data from standard to infrequent.

EFS is *elastic*, so it increases and decreases in size as necessary.

File encryption is enabled by default.
