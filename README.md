# AWS Developer Associate Notes

## Table of Contents  
[IAM](#IAM)

# <a name="IAM"/>IAM
- Consists of:
  - Users
  - Groups
  - Roles
  - Policy Documents

- Universal (does not apply to regions)
- Root account is the first account created (has complete admin access)
- New Users have NO permissions when first created
- New Users are assigned Access Key ID & Secret Access Keys when first created
- They are NOT the same as the password and cannot be used to login
- You can use them to access AWS via APIs and Command Line
- Can only view them once so save them in a secure location
- Always setup MFA for root account
- Password policies can be created
- Use Access keys to access data

- When working on development, you need to use the AWS Access keys to work with the AWS resources

# EC2
## Instances
- On Demand - Pay fixed rate by the hour (or second) with no commitment
Reserved - Capacity reservation and offers a significant discount on the hourly charge for an instance, 1 or 3 year terms

- Spot - Enables bidding for instance capacity, providing greater savings if your applications have flexible start and end times, if the instance is terminated by Amazon EC2 no charge, however you will be charge for the complete hour if you terminate it yourself

- Dedicated Hosts - Physical EC2 server dedicated for use. Can help reduce costs by allowing existing server-bound software licenses to be used

## Types
FIGHT DR MC PX!

SSD 
- General Purpose SSD - balances price and performance for a wide variety of workloads
- Provision IOPS SSD - Highest performance SSD volume for mission-critical low-latency or high-throughput workloads

Magnetic
- Throughput Optimised HDD - Low cost HDD volume designed for frequently accessed, throughput intensive workloads
- Cold HDD - Lowest cost HDD volume designed for less frequently accessed workloads
- Magnetic - Previous generation, can be a boot volume

## Load Balancers
- Application Load Balancer
- Network Load Balancer
- Classic Load Balancer

504 Error means gateway has timed out. This means that the application is not responding within the idle timeout period. Is it the Web Server or Database?

If you need the IPv4 address of your end user, look for the X-Forwarded-For header

## Route53
DNS service

Allows you to map domain names to
- EC2 Instances
- Load Balancers
- S3 Buckets

## CLI
Least Privilege - Always give your users the minimum amount of access required
Create Groups - Assign your users to groups. The users will automatically inherit the permissions of the group. The groups permissions are assigned using policy documents.

Roles allows you to not use Access Key ID's and Secret Access Keys
Roles are preferred from a security perspective
Roles are controlled by policies

# S3
## S3 - 101
- Simple Storage Service
- Object based: allows you to upload files
- Files are stored in buckets (folders)
- Not suitable to install an operating system on
- File can be from 0 bytes to 5 terabytes
- Largest PUT file = 5GB
- Unlimited storage
- Eventual consistency for overwrite PUTS and DELETES
- Successful uploads will generate a HTTP 200 status code

Storage Classes / Tiers
- S3 (durable, immediately available, frequently accessed)
- S3 - IA (durable, immediately available, infrequently accessed)
- S3 - One Zone IA (same as IA however data is stored in one availability zone)
- S3 - Reduced Redundancy Storage (easily reproducible data, thumbnails)
- Glacier - Archived data, where you can wait 3 - 5 hours before accessing

Object Fundamentals
- Key (name)
- Value (data)
- Version ID
- Metadata
- Subresources (used to manage bucket specific configuration)
(Bucket Policies, CORS, Transfer Acceleration)

## Security
- All newly created buckets are PRIVATE
- Bucket Policies (bucket level)
- Access Control Lists (object level)
- Access logs store all requests made to a bucket

## Encryption
Encryption In-Transit 
- SSL/TLS
Encryption At Rest
- Server Side (SSE-S3, SSE-KMS, SSE-C)
- Client Side

Prevent unencrypted files from being uploaded
- Use a policy which only allows x-amz-server-side-encryption parameter in the request header

Buckets are encrypted by 
- AES 256 (Advanced Encryption Standard)

## Cross Origin Resource Sharing (CORS)
- Used to enable cross origin access for your AWS resources
- E.g. accessing files located in another S3 bucket
- By default cannot be accessed
- Always use the S3 website URL not the regular bucket URL

## CloudFront
Edge Location
- Cached location, separate from an AWS region
- Not just read only, you can write to them, e.g put an object on to them

Origin
- Origin of files CDN will distribute, S3 Bucket, EC2 Instance, Elastic Load Balancer or Route53

Distribution
- Name of the CDN which consists of edge locations
- Web Distribution (websites)
- RTMP (media streaming)

Objects are cached for the life of the TTL (Time To Live)
You can clear cached objects but will be charged (Invalidation)

## Performance Optimisation
GET Intensive Workloads 
- Use CloudFront

Mixed Workloads
- Avoid sequential names for S3 objects
- Add a random prefix to the key name to prevent multiple objects being stored on the same partition
