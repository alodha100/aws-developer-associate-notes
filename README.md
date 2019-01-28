# AWS Developer Associate Notes

## Table of Contents  
- [IAM](#IAM)
- [EC2](#EC2)
- [S3](#S3)
- [Serverless-Computing](#Serverless-Computing)
- [DynamoDB](#DynamoDB)
- [KMS](#KMS)
- [Other-AWS](#Other-AWS)
- [Developer-Theory](#Developer-Theory)
- [Advanced-IAM](#Advanced-IAM)
- [Monitoring](#Monitoring)

# IAM
## 101
Error codes
- 200 = Success
- 3XX = Redirection
- 4XX = Client Error
- 5XX = Server Error

Three core services:

Kinesis Streams
- Video Streams - securely stream video from connected devices to AWS for analytics and machine learning
- Data Streams - build custom applications and process data in real-time

Kinesis Firehose
- Capture, transform, load data streams into AWS data stores for near real-time analytics with BI tools

You can configure Lambda to subscribe to a Kinesis Stream and execute a function on your behalf when a new record is detected, before sending the processed data on to its final destination
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

# Serverless-Computing
## Lambda
- Scales out (not up) automatically
- Functions are independed, 1 event = 1 function
- Serverless (like DynamoDB, S3)
- Functions can trigger other functions
- AWS X-ray allows debugging of functions
- Global, can be used to back up S3 buckets to other S3 buckets

## API Gateway
- Has caching capabilities to increase performance
- Low cost and scales automatically
- Can throttle to prevent attacks
- Results can be logged to CloudWatch
- Enable CORS for multiple domains, enforced by the client
- Use stage variables for separate environments

## Version Control
- Can have multiple versions of lambda functions
- Latest version = $latest
- Qualified versions will use $latest, unqualified will not
- Version are immutable (cannot be changed)
- Can split traffic using alias to different versions
- Cannot be split with $latest, instead create an alias

## Step Functions
- Visualises your serverless application
- Automatically triggers and tracks each step
- Logs the state of each step so can track what went wrong and where

## X-Ray
- Interceptors to add to your code to trace incoming HTTP requests
- Client handlers to instrument AWS SDK clients that your application uses to call other AWS services
- An HTTP client to use to instrument calls to other internal and external HTTP web services

Services integrates with:
- Elastic Load Balancing
- AWS Lambda
- Amazon API Gateway
- Amazon Elastic Compute Cloud
- AWS Elastic Beanstalk

Languages integrates with:
- Java
- Go
- Node.js
- Python
- Ruby
- .Net

# DynamoDB
## 101
- Low-latency NoSQL database
- Consists of Tables, Items and Attributes
- Supports both document and key-value data models
- Supported document formats are JSON, HTML, XML
- 2 types of Primary Keys: Partition Key and combination of Partition Key + Sort Key (Composite Key)

Consistency Models:
- Strongly Consistent
- Eventually Consistent

dynamodb:LeadingKeys allows users to access only the items where the partition key value matches their user ID

## Indexes
Enable fast queries on specific data columns
Provides a different view of your data based on alternative Partition / Sort Keys

Local Secondary Index
- Must be created when the table is created
- Same Partition Key as the table
- Different Sort Key

Global Secondary Index
- Can be created at any time
- Different Partition Key
- Different Sort Key

## Scans vs Queries
Query Operation
- Finds items in a table using only the primary key attribute
- Always sorted by the sort key in ascending order
- Set ScanIndexForward parameter to false to reverse the order of queries
- More efficient than scan

Scan operation
- Examines every item in the table and by default returns all data attributes
- Use the ProjectionExpression parameter to refine results 

Defining a smaller page size uses fewer read operations

## Provisioned Throughput
Capacity Units

1 x Write Capacity Unit = 1 x 1KB Write per second <br>
Size of each item / 1KB <br>
Round up <br>
Multiplied by number of write per second <br>

1 x Read Capacity Unit = 1 x 4KB Read per second <br>
Size of each item / 4KB <br>
Round up <br>
Multiplied by the number of reads per second <br>
If eventual divide by 2 <br>

# KMS
## 101
Used to create and control regional encryption keys used to encrypt data
Integrated with: EBS, S3, RDS etc

Customer Master Key (CMK)
- Alias
- Creation Date
- Description
- Key State
- Key Material (provided by customer or AWS)
- Can NEVER be exported

Setup Customer Master Key
- Create Alias and Description
- Choose material option (KMS or your own)
- Define Administrative Permissions (IAM users / roles) that can administer but not use the key through the KMS api
- Define Usage Permissions (IAM users / roles) that can use the key to encrypt and decrypt data

Commands
- aws kms encrypt
- aws kms decrypt
- aws kms re-encrypt
- aws kms enable-key-rotation

## Envelope Encryption
Customer Master Key -> decrypts -> Envelope / Data Keys 

Envelope / Data Key -> decrypts -> data

# Other-AWS
## Elastic Beanstalk
- Deploys and scales your web applications
- Supports: Java, PHP, Python, Ruby, Go, Docker, .NET and Node.js
- Application server platforms like Tomcat, Passenger, Puma and IIS
- Provisions the underlying resources for you
- Can fully manage the EC2 instances for you
- Or you can take full administrative control
- Updates, monitoring, metrics and health checks all included
- Configuration files written in YAML or JSON
- Needs .config extension and saved to the .ebextensions folder
- .ebextensions folder must be in the top level directory of your source code

Deployment Approaches

All at Once
- Service interruption while the environment is updated
- To roll back, perform another All at Once upgrade

Rolling
- Reduced capacity during deployment
- To roll back, perform another rolling update

Rolling with Additional Batch
- Maintains full capacity
- To roll back, perform another rolling update

Immutable
- Preferred option for mission critical production systems
- Maintains full capacity
- To roll back, just delete the new instances and autoscaling group

Launching an RDS instance

Within Elastic Beanstalk
- When you terminate the Elastic Beanstalk environment the database will also be terminated
- Quick and easy to add your database and get started
- Suitable for Dev and Test environments only

Outside Elastic Beanstalk
- Additional configuration steps required - Security Group and Connection Information
- Suitable for Production environments, more flexibility
- Allows connection from multiple environments, you can tear down the application stack without impacting the database

## SQS
- Distributed message queuing system
- Allows decoupling the components of an application so they are independent
- Pull-based not push-based
- Standard Queues (default): best effort ordering, message delivered at least once)
- FIFO Queues (First In First Out): ordering strictly preserved, message delivered once, no duplicates (e.g. good for banking transactions which need to happen in a strict order)
- Visibility Timeout: Default 30 seconds, increase if needed, max 12 hours
- Short Polling: Returned immediately even if no messages are in the queue
- Long Polling: Polls the queue periodically and only returns a response when a message is in the queue the timeout has been reached

## SNS
- Scalable and highly available notification service which allows you to send push notifications from the cloud
- A variety of message formats are supported: SMS text message, email, Amazon Simple Queue Service (SQS) queues, any HTTP endpoint
- Pub-sub model whereby users subscribe to topics
- Push-based not pull
- Example: a company wanting to send notifications to multiple customers could uses SNS to fan out multiple messages in SQS format using a dedicated SQS queue per customer

## SNS vs SES
- SES is for email only
- It can be used for incoming and outgoing mail
- Not subscription based, only need email

- SNS caters for various formats (SMS, SQS, HTTP, email)
- Push notifications only
- Pub/ sub model - consumers must subscribe to a topic
- You can fan out messages to a large number of recipients

## Kinesis
Three core services:

Kinesis Streams
- Video Streams - securely stream video from connected devices to AWS for analytics and machine learning
- Data Streams - build custom applications and process data in real-time

Kinesis Firehose
- Capture, transform, load data streams into AWS data stores for near real-time analytics with BI tools

You can configure Lambda to subscribe to a Kinesis Stream and execute a function on your behalf when a new record is detected, before sending the processed data on to its final destination

# Developer-Theory
## CI/CD
Continuous Integration
- Merging the code change frequently
- At least once per day
- Enables multiple devs to work on the same application

Continuous Delivery
- Automating the build, test and deployment functions

Continous Deployment
- Fully automates the entire release process
- Code is deployed into Production as soon as it has successfully passed through the release pipeline

AWS CodeCommit - Source Control Service (git)
AWS CodeBuild - Compiles source code, runs test and packages code
AWS CodeDeploy - Automated Deployment to EC2, on premises systems and Lambda
AWS CodePipeline - CI/CD workflow tool, fully automates the entire release process (build, test, deployment)

## AWS CodeCommit
- Source Control Service (GIT)
- Centralised repository for all your code, binaries, images and libraries
- Tracks and manages code changes
- Maintains version history
- Manages updates from multiple sources and enables collaboration

## AWS CodeDeploy
- Fully managed automated deployment service
- Can be used as part of Continuous Delivery or Continuous Deployment

Deployment Approaches
- In-Place or Rolling update: Application is stopped for each host as the latest code is deployed. EC2 and on premise systems only. To roll back you must re-deploy the previous version of the application

- Blue / Green - New instances are provisioned and the new application is deployed to these new instances. Traffic is routed to the new instances according to your own schedule. Supported for EC2, on-premise systems and Lambda functions. Roll back is easy, just route the traffic back to the original instances. Blue is active deployment, Green is the new release

The AppSpec file defines all the parameters needed for deployment, e.g. location of application files and pre/post deployment validation tests to run

For EC2 / On premises systems, the appspec.yml file must be placed in the root directory of your revisions. Written in YAML

Lambda supports YAML or JSON

Run Order of Hooks

- Before Block traffic -> Block Traffic -> AfterBlockTraffic
- ApplicationStop
- BeforeInstall -> Install -> AfterInstall
- Application Start
- ValidateService
- BeforeAllowTraffic -> AllowTraffic -> AfterAllowTraffic

## AWS CodePipeline
- Continuous Integration / Continuous Deliver service
- Automates end-to-end software release process based on a user defined workflow
- Can be configured to automatically trigger your pipeline as soon as a change is detected in your source code repository
- Integrates with other services from AWS like CodeBuild and CodeDeploy, as well as third part and custom plug-ins

## Docker
- Docker allows you to package up your software into containers which you can run in Elastic Container Service (ECS)
- A Docker Container includes everything the software needs to run including code, libraries, runtime and environment variables etc
- We use a special file called a Dockerfile to specify the instructions needed to assemble your Docker image
- Once built, Docker images can be stored in Elastic Container Registry (ECR) and ECS can use the image to launch Docker containers
- Docker Commands to build, tag (apply and alias) and push your docker image to the ECR repository

docker build -t
docker tag
docker push

## AWS CodeBuild
- Fully managed build service, can build source code, run tests and produce software packages based on commands that you define yourself
- By default the buildspec.yml defines the build commands and settings used by CodeBuild to run your build
- You can override the settings in buildspec.yml by adding commands in the console when the build is launched
- If the build fails, check the build logs in the CodeBuild console and you can also view the full CodeBuild log in CloudWatch

# Advanced-IAM
## Web Identity Federation
- Federation allows users to authenticate with a Web Identity Provider (Google, Facebook, Amazon)
- The user authenticates first with the Web ID provider and receives an authentication token, which is exchanged for temporary AWS credentials allowing them to assume an IAM role

## Cognito
- Cognito is an Identity Broker which handles interaction between your applications and the Web ID provider (no code required)
- Provides sign-up, sign-in and guest user access
- Syncs user data for a seamless experience across your devices
- Cognito is the AWS recommended approach for Web ID Federation particularly for mobile apps
- Uses User Pools to manage user sign-up and sign-in directly or via Web Identity Providers
- Acts as an Identity Broker, handling all interaction with Web Identity Providers
- Uses Push Synchronisation to send a silent push notification of user data updates to multiple device types associated with a user ID

## Policies
- Managed Policy - AWS-managed default policy
- Customer Managed Policy - Managed by you
- Inline Policy - Managed by you and embedded in a single user, group or role

# Monitoring
## CloudWatch
CloudWatch is a monitoring service to monitor your AWS resources, as well as the application that you run on AWS

Host Level Metrics:
- CPU
- Network
- Disk
- Status Check

Ram Utilisation 
- Custom metric

Custom Metrics
- Minimum granularity is 1 minute

Terminated Instances
- You can retreive data from any terminated EC2 or ELB instance after its termination. 
- CloudWatch Logs by default are stored indefinitely

Metric Granularity
- 1 minute for detailed monitoring
- 5 minutes for standard monitoring

CloudWatch can be used on premise - Not restricted to just AWS resources. Just need to download and install the SSM agent and CloudWatch agent

## CloudWatch vs CloudTrail
- CloudWatch monitors performance
- CloudTrail monitors API calls in the AWS platform
- AWS Config records the state of your AWS environment and can notify you of changes
