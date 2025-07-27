+++
title = 'AWS Notes'
date = 2024-02-11

+++

### Edx Course

- AZ - Availability zone: A physically distinct, independent infrastructure, that is engineered to be highly reliable.
- AZ scope : EC2
- The virtual server is known as an EC2 instance. It runs on a physical host that is inside an AWS AZ. There will be two or more AZ within an AWS Region.
- IAM : allows to create users, who can interact with AWS Resources
- The network in AWS is provided by Amazon VPC. One can create a VPC within an AWS region and within that VPC you define subnets to manage related sets of servers or other AWS resources.
- VPC lets you define rules for how network traffic from your subnets is routed. You can also decide whether your network should be connected to the Internet, to corporate networks, or to keep the network completely private
- Create Public subnet and private subnet. In public subnet we keep web server and in private DB server.
- A VPC has region scope. And subnets have AZ scope.
- Security in AWS is a shared responsibility between AWS and the customer. For more see [Share Responsibilty Model](https://aws.amazon.com/compliance/shared-responsibility-model/)
- **CloudFormation** : An AWS service that can take in a decalrative document called a 'template' and use it to provision AWS resources on your behalf so you don't have to.
- **EC2 Metadata service** : This is a service that intercepts calls to 169.254.169.254 from your EC2 instance to communicate metadata to the instance.
- Properties that can be retrieved are documented here [EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)

### Invoking AWS APIs

- An AWS IAM user is an entity that you create in AWS to represent the person or service that uses it to interact with AWS. You attach permission policies to the IAM user that determine what the user can and cannot do in AWS.
- Access keys are a combination of an access key ID and a secret access key that are assigned to a user
- When you create IAM policies, follow the standard security advice of granting least privilege - that is, granting only the permissions required to perform a task.
- Determine what users need to do and then craft policies for them that let the users perform only those tasks.
- It is a security best practice to remove IAM user credentials that are not needed. After this exercise, make sure to remove the access keys only (not the AWS Console password) for the IAM user
- API requests are typically _signed_ with an [access key](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys) belonging to an IAM user and using the [Signature Verson 4 Signing Process](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html). It is also possible to sign these API requests using [temporary security credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) such as those derived from an IAM Role.
- AWS SDKs check several locations for credentials such as local configuration files and environment variables. If the SDK finds credentials, it will automatically sign your API requests for you. For example, here is how the [Python SDK checks for Credentials](https://boto3.readthedocs.io/en/latest/guide/configuration.html)

### Amazon S3

- First we create a bucket. It is the fundamental storage unit in S3. A bucket lives in a region.
- Objects are the fundamental entities stored in buckets.
- Access Control can be done using : Policies (JSON), IAM Users & Groups (we can attach a policy for a user or group), Access Control Lists (List, Write and Read privileges to a back)
- Data is already replicated in S3
- [Amazon Simple Storage Service](https://docs.aws.amazon.com/AmazonS3/latest/dev/Welcome.html) is object storage built to store and retrieve any amount of data from anywhere.
- You can store files as [Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingObjects.html) within [S3 Buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html)
- S3 allows you to use Bucket Policies, IAM Policies, and ACLs to grant permissions to the contents of the bucket.
- You can use Presigned URLs for time-limited access to objects.
- The [S3 Service Feature Guide for Boto3](https://boto3.readthedocs.io/en/latest/guide/s3.html) contains Python examples of working with S3. In particular, you should review the section on [Generating Presigned Urls](https://boto3.readthedocs.io/en/latest/guide/s3.html#generating-presigned-urls)

### Application Flow

- The application is deployed on an Amazon EC2 instance with an Application Load Balancer sitting in front of the instance to direct user requests to the instance
- Amazon Cognito is used to sign up/sign in users for the application
- In order to asynchronously process the photo labels, when a photo is uploaded, an Amazon S3 bucket event notification is issued to a SNS topic. This triggers a subscribed Lambda function, which talks to Amazon Rekognition
- To make the application more distributed, a SQS queue subscribed to the Amazon SNS topic stores all the incoming requests and an on-premise application polls the queue for processing
- AWS X-Ray traces the calls made to all the AWS resources in this application, thereby providing diagnostics information.

#### Amazon Rekognition

- [Amazon Rekognition](https://docs.aws.amazon.com/rekognition/latest/dg/what-is.html) is a service that applies deep learning to analyze the contents of images and videos. It supports functionality such as scene detection, face detection, even celebrity recognition.
- [Detect Labels](http://boto3.readthedocs.io/en/latest/reference/services/rekognition.html#Rekognition.Client.detect_labels) functionality takes an image as input and returns labels with confidence values such as `{Name: lighthouse, Confidence: 98.4629}`

### AWS IAM

- [AWS Identity and Access Management (IAM)](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)
- [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [IAM Roles for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html)
- When you log in to AWS using your email address and password, you are authenticating as the [Root User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) for the account.
- The best practice is to avoid logging in as Root except for a [handful of operations that only the root user can perform](https://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html)
- You can create [IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html) within your AWS Account for yourself and for any others that need access to resources in your account.
- Permissions are granted or denied with [IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)
- If you find that you'll have several users who need similar permissions, define an [IAM Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html) and associate your users to that group.
- When working with AWS Services, you may also encounter [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html)
- Many AWS services require that you use roles to control what that service can access.
- IAM Roles provide only _temporary security credentials_. One common case is allowing the EC2 service to distribute credentials to your application code running on an EC2 instance. Roles can also enable other scenarios in the enterprise such as cross-account access and identity federation.
- (OpenId connect) OIDC identity provider
- IAM OIDC IdP are entities in IAM that describe an external identity provider service that supports the OIDC standard.
- You use IAM OIDC IdP when you want to establish trust b/w an OIDC-compatible IdP and your AWS account.

### Load Balancing

- Network config for listening and availability zones.
- Register EC2 instances
- Configure health checks
- Start using LB host name in DNS
- `zip -r ~/deploy-app.zip Deploy/ FlaskApp/`

### Pricing Models

- On-demand Instance Pricing : leasing the server at the rates we agree to. No commit model. Pay a little bit more.
- Reserved Instance Pricing : Pay down payment now on a 1 or 3 year term. What Amz does then is lower the on-demand prices.
- Spot instance pricing : is trying to outbid others for processing power. Bidding on the leftover instances that no body has claimed.

### Instance Types

- Instance types always include a mix of:
  - Memory
  - Processing Power (ECU)
  - Storage (EBS)
  - I/O performance
- Instance families
  - Micro
  - Standard
  - High-Memory
  - High-CPU
  - Cluster-Compute / GPU
- ECU - EC2 Compute Units. It is equivalent of 1.0 to 1.2 GHz 2007 Xeon Processor

### AWS Storage Services

- **AWS Glacier** - like S3 but not readily accessible. Should be used to archived data. One can create a set up (lifecycle rule on a bucket) to migrate all data from S3 to Glacier for archiving.
- **EFS** - Elastic File System : Network Attached Storage. It is specifically for EC2 servers. This allows multiple servers to access one datasource.
- **Storage Gateway** : enables hybrid storage b/w on-premise envs and the AWS Cloud. Provides a low latency performance by caching frequently used data on-premises. While storing less frequently used data in AMZ Cloud Storage services.
- **Snowball device** : portable, petabyte scale data storage device that can be used to migrate data from on-premise env to AWS Cloud.

### EC2 on AWS

- [AMI UserGuide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html)
- [Create an Amazon EBS-backed Linux AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/creating-an-ami-ebs.html)
- [EC2 Image Builder](https://docs.aws.amazon.com/imagebuilder/latest/userguide/what-is-image-builder.html)
- EC2 instances are a combination of virtual processors (vCPUs), memory, network, and in some cases, instance storage and GPUs.
- Instance types consist of a prefix identifying the type of workloads they’re optimized for, followed by a size. For example, the instance type **c5.large** can be broken down into the following elements.
  - **c5** determines the instance family and generation number. Here, the instance belongs to the fifth generation of instances in an instance family that’s optimized for generic computation.
  - **large**, which determines the amount of instance capacity

| Instance family       | Description                                                                                                                                                                                                                                                                                  | Use Cases                                                                                                                                                                                                |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| General purpose       | Provides a balance of compute, memory, and networking resources, and can be used for a variety of workloads.                                                                                                                                                                                 | Scale-out workloads such as web servers, containerized microservices, caching fleets, distributed data stores, and development environments.                                                             |
| Compute optimized     | Ideal for compute-bound applications that benefit from high-performance processors.                                                                                                                                                                                                          | High-performance web servers, scientific modeling, batch processing, distributed analytics, high-performance computing (HPC), machine/deep learning, ad serving, highly scalable multiplayer gaming.     |
| Memory optimized      | Designed to deliver fast performance for workloads that process large data sets in memory                                                                                                                                                                                                    | Memory-intensive applications such as high-performance databases, distributed web-scale in-memory caches, mid-size in-memory databases, real-time big-data analytics, and other enterprise applications. |
| Accelerated computing | Use hardware accelerators or co-processors to perform functions such as floating-point number calculations, graphics processing, or data pattern matching more efficiently than is possible with conventional CPUs.                                                                          | 3D visualizations, graphics-intensive remote workstations, 3D rendering, application streaming, video encoding, and other server-side graphics workloads.                                                |
| Storage optimized     | Designed for workloads that require high, sequential read and write access to large data sets on local storage. They are optimized to deliver tens of thousands of low-latency random I/O operations per second (IOPS) to applications that replicate their data across different instances. | NoSQL databases, such as Cassandra, MongoDB, and Redis, in-memory databases, scale-out transactional databases, data warehousing, Elasticsearch, and analytics.                                          |

- [Reliability Pillar](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/welcome.html)
- [Instance lifecycle](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-lifecycle.html)
- [EC2 pricing](https://aws.amazon.com/ec2/pricing/)
- [On-Demand EC2 pricing](https://aws.amazon.com/ec2/pricing/on-demand/)
- [Spot EC2 pricing](https://aws.amazon.com/ec2/spot/pricing/)
- [Reserved Instances pricing](https://aws.amazon.com/ec2/pricing/reserved-instances/pricing/)

### Containers on AWS

- If you’re trying to manage your compute at a large scale, you need to know:
  - How to place your containers on your instances?
  - What happens if your container fails?
  - What happens if your instance fails?
  - How to monitor deployments of your containers?
- This coordination is handled by a container orchestration service. AWS offers two container orchestration services: Amazon Elastic Container Service (ECS) and Amazon Elastic Kubernetes Service (EKS).
- To run and manage your containers, you need to install the Amazon ECS Container Agent on your EC2 instances
- An instance with the container agent installed is often called a _container instance_.
- To prepare your application to run on Amazon ECS, you create a task definition. The task definition is a text file, in JSON format, that describes one or more containers. A task definition is similar to a blueprint that describes the resources you need to run that container, such as CPU, memory, ports, images, storage, and networking information.
- Amazon EKS is conceptually similar to Amazon ECS, but there are some differences.
  - An EC2 instance with the ECS Agent installed and configured is called a container instance. In Amazon EKS, it is called a worker node.
  - An ECS Container is called a task. In the Amazon EKS ecosystem, it is called a pod.
  - While Amazon ECS runs on AWS native technology, Amazon EKS runs on top of Kubernetes.
- Amazon ECS and Amazon EKS enable you to run your containers in two modes.
  - Amazon EC2 mode
  - AWS Fargate mode
- **Task** - The lowest level building block of ECS - Runtimes instances
- **Task Definitions** - Templates for your **Tasks**. This is where we specify our Docker image, Memory/CPU requirements, etc.
- **Container (EC2 Only)** - Virtualized Instance that run your **Tasks**
- **Cluster**
  - EC2 - A group of Containers which run **Tasks**
  - Fargate - A group of **Tasks**
- **Service** - A Task management system that ensures X amount of tasks are up and running.
- [Containers](https://aws.amazon.com/containers/services/)
- [What is Container](https://www.docker.com/resources/what-container)
- [Amazon ECS Agent](https://github.com/aws/amazon-ecs-agent)
- [Container Instances](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_instances.html)

### Serverless on AWS

- Every definition of serverless mentions four aspects:
  - No servers to provision or manage.
  - Scales with usage
  - You never pay for idle resources
  - Availability and fault tolerance are built-in
- [Serverless on AWS](https://aws.amazon.com/serverless/)
- [Serverless Getting started](https://aws.amazon.com/serverless/getting-started/?serverless.sort-by=item.additionalFields.createdDate&serverless.sort-order=desc)
- [Serverless architecture](https://aws.amazon.com/lambda/serverless-architectures-learn-more/)
- [Best practices for serverless](https://aws.amazon.com/blogs/compute/best-practices-for-organizing-larger-serverless-applications/)
- [Configuring AWS lambda functions](https://docs.aws.amazon.com/lambda/latest/dg/lambda-functions.html)
- [10 things Serverless Architects Should know](https://aws.amazon.com/blogs/architecture/ten-things-serverless-architects-should-know/)
- [AWS Alien Attack: A Serverless Adventure](https://catalog.us-east-1.prod.workshops.aws/v2/workshops/fc36cb5a-de1b-403f-84dd-cc824390c548/en-US)

### Networking on AWS

- [VPCs and Subnets](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Subnets.html)
- [Default VPC and default subnets](https://docs.aws.amazon.com/vpc/latest/userguide/default-vpc.html)
- A VPC is an isolated network you create in the AWS cloud, similar to a traditional network in a data center. When you create a VPC, you need to choose three main things.
  - The name of your VPC
  - A Region for your VPC to live in. Each VPC spans multiple Availability Zones within the Region you choose.
  - A IP range for your VPC in CIDR notation. This determines the size of your network. Each VPC can have up to four /16 IP ranges
- After you create your VPC, you need to create subnets inside of this network. Think of subnets as smaller networks inside your base network—or virtual area networks (VLANs) in a traditional, on-premises network.
- When you create a subnet, you need to choose three settings.
  - The VPC you want your subnet to live in, in this case VPC (10.0.0.0/16).
  - The Availability Zone you want your subnet to live in.
  - A CIDR block for your subnet, which must be a subset of the VPC CIDR block, in this case 10.0.0.0/24.
- To enable internet connectivity for your VPC, you need to create an internet gateway. Think of this gateway as similar to a modem. Just as a modem connects your computer to the internet, the internet gateway connects your VPC to the internet
- A virtual private gateway allows you to connect your AWS VPC to another private network. Once you create and attach a VGW to a VPC, the gateway acts as anchor on the AWS side of the connection
- [VPC with public and private subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenario2.html)
- [Managing route tables for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html#CustomRouteTables)
- [Customer Gateway](https://docs.aws.amazon.com/vpn/latest/s2svpn/how_it_works.html#CustomerGateway)
- [What is Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
- [Overview of VPCs and subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)
- When you create a VPC, AWS creates a route table called the main route table. A route table contains a set of rules, called routes, that are used to determine where network traffic is directed
- Think of a network ACL as a firewall at the subnet level. A network ACL enables you to control what kind of traffic is allowed to enter or leave your subnet.
- Network ACL’s are considered stateless, so you need to include both the inbound and outbound ports used for the protocol
- [Example routing options](https://docs.aws.amazon.com/vpc/latest/userguide/route-table-options.html)
- [Work with route tables](https://docs.aws.amazon.com/vpc/latest/userguide/WorkWithRouteTables.html)
- [Control traffic to subnets with Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)
- [Control traffic to EC2 instances with security groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)
- [Allow users to connect on HTTP or HTTPS](https://repost.aws/knowledge-center/connect-http-https-ec2)

### Storage on AWS

- AWS storage services are grouped into three different categories: **block storage**, **file storage**, and **object storage**.
- Block storage in the cloud is analogous to **direct-attached storage (DAS)** or a **storage area network (SAN).**
- File storage systems are often supported with a **network attached storage (NAS)** server.
- [Cloud Storage](https://aws.amazon.com/what-is/cloud-storage/)
- [Types of Cloud Storage](https://aws.amazon.com/what-is/object-storage/#types)
- EC2 Instance Store provides temporary block-level storage for your instance. This storage is located on disks that are physically attached to the host computer.
- As the name implies, Amazon EBS is a block-level storage device that you can attach to an Amazon EC2 instance. These storage devices are called Amazon EBS volumes. Like external HDD to laptop.
- [Amazon EBS](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html)
- [EBS FAQs](https://aws.amazon.com/ebs/faqs/)
- [Storage](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Storage.html)
- [Cloud Storage on AWS](https://aws.amazon.com/products/storage/)
- [Amazon EFS : How it works](https://docs.aws.amazon.com/efs/latest/ug/how-it-works.html)
- [Amazon FSx for Windows File Server](https://aws.amazon.com/fsx/windows/)
- [Amazon FSx for Lustre](https://aws.amazon.com/fsx/lustre/)

### Datbases on AWS

- [What is a Relational Database](https://aws.amazon.com/relational-database/)
- [AWS Cloud Databases](https://aws.amazon.com/products/databases/)
- [Working with backups](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithAutomatedBackups.html)
- [Amazon RDS backup and restore](https://aws.amazon.com/rds/features/backup/)
- [Creating and using an IAM policy for IAM database access](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.IAMDBAuth.IAMPolicy.html)
- [Amazon Virtual Private Cloud VPCs and Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.html)
- In DynamoDB, tables, items, and attributes are the core components that you work with. A table is a collection of items, and each item is a collection of attributes. DynamoDB uses primary keys to uniquely identify each item in a table and secondary indexes to provide more querying flexibility.
- [Amazon Dynamo DB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)
- [AWS Database Blog](https://aws.amazon.com/blogs/database/?nc=sn&loc=4)
- [Database Freedom](https://aws.amazon.com/products/databases/freedom/?nc=sn&loc=5)

### Monitoring on AWS

- [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/)
- Each metric in CloudWatch has a timestamp and is organized into containers called **namespaces**. Metrics in different namespaces are isolated from each other — you can think of them as belonging to different categories.
- AWS services that send data to CloudWatch attach **dimensions** to each metric. A dimension is a name/value pair that is part of the metric’s identity. You can use dimensions to filter the results that CloudWatch returns. For example, you can get statistics for a specific EC2 instance by specifying the InstanceId dimension when you search.
- **Log event**: A log event is a record of activity recorded by the application or resource being monitored, and it has a timestamp and an event message.
- **Log stream**: Log events are then grouped into log streams, which are sequences of log events that all belong to the same resource being monitored. For example, logs for an EC2 instance are grouped together into a log stream
- **Log groups**: Log streams are then organized into log groups. A log group is composed of log streams that all share the same retention and permissions settings.
- [Getting Started with Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/GettingStarted.html)
- [What is Amazon CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html)
- [AWS services that publish CloudWatch metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/aws-services-cloudwatch-metrics.html)
- [Viewing Available Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/viewing_metrics_with_cloudwatch.html)
- [CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)
- [Amazon SNS](https://aws.amazon.com/sns/?whats-new-cards.sort-by=item.additionalFields.postDateTime&whats-new-cards.sort-order=desc)

### Optimization

- Types of High Availability
  - _Active-Passive_ : With an active-passive system, only one of the two instances is available at a time. One advantage of this method is that for stateful applications where data about the client’s session is stored on the server, there won’t be any issues as the customers are always sent to the same server where their session is stored.
  - _Active-Active_ : A disadvantage of active-passive and where an active-active system shines is scalability. By having both servers available, the second server can take some load for the application, thus allowing the entire system to take more load. However, if the application is stateful, there would be an issue if the customer’s session isn’t available on both servers. Stateless applications work better for active-active systems.
- [High Availability and Scalability on AWS](https://docs.aws.amazon.com/whitepapers/latest/real-time-communication-on-aws/high-availability-and-scalability-on-aws.html)
- [Elastic Load Balancing Product Comparisons](https://aws.amazon.com/elasticloadbalancing/features/#Product_comparisons)
- [AWS Certificate Manager](https://aws.amazon.com/certificate-manager/)
- [Authenitcate Users using an ALB](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/listener-authenticate-users.html)
- [How AWS WAF works](https://docs.aws.amazon.com/waf/latest/developerguide/how-aws-waf-works.html)
- [Amazon EC2 Auto Scaling](https://aws.amazon.com/ec2/autoscaling/)
- [Autoscaling FAQs](https://aws.amazon.com/ec2/autoscaling/faqs/)
- [Setting capacity limits on your ASG](https://docs.aws.amazon.com/autoscaling/ec2/userguide/asg-capacity-limits.html)
- [Step and simple scaling policies for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-scaling-simple-step.html)
- [Target tracking scaling policies for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-scaling-target-tracking.html)
- [Creating an Auto Scaling group using a launch template](https://docs.aws.amazon.com/autoscaling/ec2/userguide/create-asg-launch-template.html)

### Addressing Security Risks

- [IAM tutorial: Delegate access across AWS accounts using IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html)
- [AWS Organizations](https://aws.amazon.com/organizations/)
- A route table contains a set of rules, called routes, that are used to determine where network traffic is directed.
- Each subnet in your VPC must be associated with a route table; the table controls the routing for the subnet
- [Encryption of data in transit](https://docs.aws.amazon.com/whitepapers/latest/efs-encrypted-file-systems/encryption-of-data-in-transit.html)
- [Bucket encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-encryption.html)
- [Securing EC2](https://aws.amazon.com/answers/security/aws-securing-ec2-instances/)
- [Lambda Security](https://d1.awsstatic.com/whitepapers/Overview-AWS-Lambda-Security.pdf)
- [Elastic Load Balancing](https://aws.amazon.com/elasticloadbalancing/)
- [TLS Termination](https://aws.amazon.com/blogs/aws/new-tls-termination-for-network-load-balancers/)
- [Amazon API Gateway](https://aws.amazon.com/api-gateway/)
- Based on five pillars — operational excellence, security, reliability, performance efficiency, and cost optimization

### Migrating to the Cloud

- [6 Strategies for Migrating Applications to the Cloud](https://aws.amazon.com/blogs/enterprise-strategy/6-strategies-for-migrating-applications-to-the-cloud/)
- [How to migrate](https://aws.amazon.com/cloud-migration/how-to-migrate/)
- Checkout these [Elastic Beanstalk tutorials](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/tutorials.html) and [concepts](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/concepts.html) for more info.
- Here's more info about the [Security pillar](https://wa.aws.amazon.com/wat.pillar.security.en.html). You can also checkout the following articles to learn about how to protect your data [in transit](https://wa.aws.amazon.com/wat.pillar.security.en.html) and at [rest](https://wa.aws.amazon.com/wat.question.SEC_9.en.html)
- [AWS Migration Hub](https://aws.amazon.com/migration-hub/faqs/) provides a rich web experience that helps you understand your existing IT assets as well as view and track progress of application migrations. It lets you collect and view data about on-premises resources, group those resources into applications, and track the progress of those applications as you migrate to AWS.
- [Database Migration - What do you need to know before you start](https://aws.amazon.com/blogs/database/database-migration-what-do-you-need-to-know-before-you-start/)
- [Migration Delivery Partners](https://aws.amazon.com/migration/partner-solutions/) help customers through every stage of migration, accelerating results by providing personnel, tools, and education in the form of professional services
- SSD : General Purpose and Provisioned IOPS.
- HDD : Throughput optimized and Cold
- Data Transfer
  - [A​WS Storage Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/WhatIsStorageGateway.html)
  - [AWS DataSync](https://docs.aws.amazon.com/datasync/latest/userguide/how-datasync-works.html)
  - [AWS Snowball](https://docs.aws.amazon.com/snowball/latest/ug/snowball-transfer-client.html)
  - [AWS Snowball FAQs](https://aws.amazon.com/snowmobile/faqs/)
- [AWS Server Migration Service (AWS SMS)](https://docs.amazonaws.cn/en_us/server-migration-service/latest/userguide/server-migration.html) automates the migration of your on-premises VMware vSphere, Microsoft Hyper-V/SCVMM, and Azure virtual machines to the AWS Cloud. AWS SMS incrementally replicates your server VMs as cloud-hosted Amazon Machine Images (AMIs) ready for deployment on Amazon EC2. Checkout [AWS Server Migration Service FAQs](https://aws.amazon.com/server-migration-service/faqs/) for more info.
- [Overview of Amazon Aurora](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html)
- [How aurora serverless works](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-serverless.how-it-works.html)
- With AWS DMS, you can perform one-time migrations, and you can replicate ongoing changes to keep sources and targets in sync. Checkout [How AWS Database Migration Service Works](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Introduction.html) for more info. If you want to change database engines, you can use the [AWS Schema Conversion Tool (AWS SCT)](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Welcome.html) to translate your database schema to the new platform. You then use AWS DMS to migrate the data.
- Tools like the [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html), [AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html), and [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-whatis-howdoesitwork.html) all provide methods for you to automate actions being taken while not losing control over what you are doing.
- Here are additional resources that you might want to checkout:
  - [Working with SSM Agent](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html)
  - [A​WS CloudFormation Templates](https://aws.amazon.com/cloudformation/aws-cloudformation-templates/)
- AWS provides many tools to give you more of a hands-off approach to your migration tasks. Whether handling assessment, planning, or full application migration, there are several tools within AWS to utilize.
  - [Mi​grate to AWS with Confidence](https://aws.amazon.com/cloud-migration/#tools-and-services)
  - [T​SO Logic](https://tsologic.com/)
  - [C​loudEndure](https://www.cloudendure.com/live-migration/aws/)

### Random

- With EventBridge we can integrate with 3rd party services, Shopify, Pagerduty etc.
- For a specific rule we can have maximum 5 targets
- Internet Gateway and VPC have one to one mapping.
- NAT Gateways only allow outbound requests
- **StepFunctions** Allow you to create workflows that follow a fixed or dynamic squeuence - aka "steps" and has built in retry functionality - don't progress until success.
