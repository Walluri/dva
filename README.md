      `# DVA

## Global Services
1. IAM
2. DNS
3. Cloudfront

## IAM

1. We create users and asign them to group/'s'
2. David and Shawn - Developers Group
3. Mike and MAry - Testers grop

4. We also attach policies (permissions) to users or groups.
5. If we see this in the top it means you have logged in as a user created by the root account.
- Account ID: 1223-1233-1234
- IAM user: iamadmin
- And the account name will be displayed as : iamanuser @ da1general
6. IAM Policy Structure
  - Effect : Allow or Deny
  - Principal : which user
  - Action : "s3:GetObject"
  - Resource: ["arn:aws:s3:::mybucket/*"]
 
7. You can go to an IAM user and add permissions to that user,
   - By, selecting an AWS managed policy
   - By, attaching a Customer managed policy
   - By, creating an inline policy.

8. AWS Access mechanism
   - Management console - username/password/MFA
   - CLI - via  Access Keys
   - SDK - In application code via Access keys [For Example Javascript SDK will be embeded into your application code]
   - The AWS cli that we use is built on top of AWS SDK for Python i.e Boto
   - Some aws cli commands
   - - aws configure --profile profileabc
     - aws configure list-profiles
    
9. IAM Roles for Services
    - Some AWS services on our AWS account will need to perform certain actions on certain resources on our behalf.
    - So we need to assign permissions to AWS services - so we create an IAM role.
    - These roles are intended to be used by AWS services.
      
10. While creating a role..
    - We select a trusted entity type. [service like EC2 or Lambda / other 4 entities ]
    - We assign permissions or policies.

## Billing
1. Normally the billing dashboard [when you click on the tip right ] will only be accessible to the root user.
2. Even if you have administrator permissions, you wont be able to see them billing details like which service took how much and the grand total bill etc
3. So After we login with root user - go to billing dashboard - and explicitly say that Iam users can also access the billing dashboard. [Then the users with the right permissions can access the billing details.]

## EC2
1. Consists of 4 main functionalities.
   - Creating Virtual Machines : EC2
   - Storing Data on virtual drives : EBS
   - Distributing load across machines : ELB
   - Scaling the services using an Auto scaling group (ASG).
2. EC2 User Data
   - bootstraping instance means - launching commands when the machine starts.
   - This can be done with **EC2 User data** script.
   - when does this script run ? : It runs only once at the instance **first launch**.
   - What do you want to do in the script ? : install updates, install software, download files from the internet.
   - **Note** : EC2 user data script runs with the root user.
  
3. Key pair
   - Type : [RSA encrypted private and public key pair / ED25519]
   - Private key file format : [ .pem(OpenSSh), .ppk(PuTTY)]

4. Security Group : 
    - They are like a firewal outside and EC2 instance.
    - And, They Control traffic from and to the EC2 instance. So we can add rules.
    - They are the fundamental of network security in AWS.
    - Security groups only contain ALLOW rules. So we only say what the is ALLOWED to go or to go out.
    - This means we can't explicitly deny.
    - The rule can be mentioned with Ip address / or by other security groups.
    - **Note** : Why are security groups locked down to a VPC ?
    - When trying to create a security group, There will be default rule in the outbound rules : 
      Type : All Traffic [Protocol : All]
      Port Range : All
      Destination: 0.0.0.0/0
      This means all kinds of outgoing traffic is allowed, by default.
    - Refer Other security group.
      Lets say, you have an EC2 instanceA with inbound rules via Security Group 1 and Security Group 2.
      If there is other EC2 instanceB which has a security group 2, Then we dont have to explicitly get the IP of instanceB if instanceA wants to allow traffic from instanceB.
    - **Note** : When ever there is a timeout while accessing a particular service in EC2 instance : It means there is an issue with the security group.

5. How do you connect to EC2 instance to perform maintenance activity.
   - SSH : port 22 via public ip + pem file.
   - EC2 instance connect : A browser based SSh connection into EC2 instance.A temporary SSh key will be uploaded by AWS via the browser.
   - Amazon ami has one user already setup - ec2-user + it comes with the aws cli.
   - Using the aws cli - you can run aws configure and update you access key id and secret access key.But that is a bad idea as other users who ssh to your machine can hands on them.
   - Alternatively use IAM roles.
   - **Note:** : 43 (EC2 Instance purchainsg options ?? needed for exam?

6. IP: After you stop and start an instance -  Th public ip changes the private ip does not change.

7. **Note** : Instance types : Do we have to read

8. EC2 Instance Storage options.
   - EBS : Elastic Block Store which you can attach to your instances while they are running. [like a usb stick]
   - Good : The data is persisted even after the EC2 instance is terminated.
   - Bad : They are locked to **AZ** (like EC2 instances) and can be attached to only one EC2 instance at a time.
   - EBS volume is like a network drive and so there might be latency, but detached to one instance and attached to another instance.
   - GB and IOPS have to be provisioned in advance, and you will be billed for what you have provisioned.
   - There is a root volume associated with EC2 instance, you can configure that it should not be deleted when EC2 is terminated.

9. EBS snapshots
    - Its a backup of the EBS volume, for disaster recovery.
    - We can copy this EBS snapshots across AZ or Regions.
    - Save Money : You can move a snapshot to an archive tier which is 75% cheaper, with 24 - 72 hours to restore.
    - Accidental deletion : You can still recover the deleted one with in 1 year, by setting a retention period from 1D-1Y.
    - Restore data from Snapshot quickly? : FSR, Fast snapshot restore, but $$$.
    - You can create snapshots from volumes and create volumes from snaoshots.
      
10. EC2 Instance store.
    - We can attach network drives on to our EC2 instances, EBS volumes - they have limited performance.
    - EC2 Instance store is an hard disk attached to the EC2 instance.
    - When do you need them : For better I/O performance.
    - Persistence ? : When you stop or terminate your EC2 instance - the storage will be lost. [**ephemeral storage ?? **]
    - Risk of data loss : If hardware fails.

11. **EBS volume types**
    GP2 / GP3
    IO1/IO2

12. IO1 / IO2 family with multi attach.
    - We can attach the same EBS volume to multiple EC2 instances, with in AZ
    - Not more than 16 instances at a time.

13. **Revisit and do hands on: Amazon EFS** : Elastic File system.
    - This is a managed NFS.
    - This can be mounted into **many** EC2 instances, which can be in different AZ's. Ex 1000s of cuncurrent NFS clients with 10GB+ / sec throughput.
    - Highly available
    - Scalable
    - Expensive : 3 times GP2
    - But, we pay per use.
    - **USE cases** : content management / Data sharing / web serving / wordpress
    - Internally uses the NFS protocol to communicate.
    - Uses Secutiry Goup to control accessto EFS.
    - Compatible with only linux based AMI's.

14.  EBS vs EFS
     - EBS : EBS volumes attach to one instance at a time expect when we use the multi attach feature of the io1 / io2
     - EBS volumes are locked at AZ level
     - EBS : For GP2 volumes the IO increases if the disk size increases
     - EBS : For IO1 type volumes the IO can increase independently.
     - EBS : To migrate a volume we take snapshots.
     - EBS : When running backups they consume a lot of IO, so do not do the backup's when you have high traffic.
     - EBS : Root EBS volumes of instances gets terminated by default along with the EC2 instance.

     - EFS : ITs a network file system and we can attach it to 100's of 'LINUX' instances in one particular Region.
     - EFS : costlier thatn EBS
     - EFS : Can use the IA feature for cost savings.
   
## ELB + ASG
1. Load balancing : A load balancer is a server or a set of servers that will forward received traffic to multiple downstream multiple instances.
2. Why a load balancer ?
   - Spread load across multiple instances.
   - Single point DNS access to our application.
   - Seamlessly handle failure to downstream instances, based on health check performances [port + route ].
   - Provide SSL Termincation for websites **??**
   - Enforce **stickyness** with cookies.
   - High availability across AZ's.
   - AWS will manage this load balancer.
   - AWS Load balancer can be integrated with :  EC2, EC2 Autoscaling groups,  **ECS**, Route53, WAF etc
3. Types of load balancers.
   -   Classic load balancer : HTTP, HTTPS, TCP, SSL.
   -   Application load Balancer : HTTP, HTTPS, Websocket
   -   Network Load Balancer : TCP, TLS, UDP
   -   Gateway Load Balancer : Layer 3 - IP Protocol.
   -   Some load balancers can be set up as public / private too.
4. Security of load balancers - Application Load Balancer.
   -  Its a layer 7 load balancer (Http)
   -  It allows you to route to traffic to multiple applications across machines[machines will be grouped into target groups].
   -  We can even route traffic to multiple applications in the same machine Ex: containers
   -  **Support for HTTP2 and websocket ?**
   -   **Redirect traffic from HTTP to HTTPS**
5. Routing - Application Load Balancer.
   - Routing based on path in url : example.com/users OR example.com/posts
   - Routing based on hostname in url : one.example.com and two.example.com
   - Routing based on Query String OR even Headers.
   - ALB's have a port mapping feature to redirect traffic to a dynamic port in ECS, they are a good fit for container based applications.
6. Target Groups : Application load balancer.
   - EC2 Instances :
   - ECS Tasks.
   - Lambda functions.
   - ALB can even route traffic to private IP's
7. Good things : ALB
   - We get a fixed hostname with ALBs : abc.region.elb.amazonaws.com
   - The application servers can see the the below details in the headers 
        X-Forwarded-For: IP
        X-forwarded-port : port
        X-forwarded-proto : protocol
8. Network Load Balancer:
   - Operates at layer 4.
   - Forwards TCP and UDP traffic
   - millions of requests per second, with a latency of ~100ms
   - If you want to be accessed with IPs ( static ip ) - NLB has one Static IP per AZ. And we can assign an elastic ip.
   - **Target groups : EC2 instances , Private IP address also**
   - There are instances where people use NLB to route traffic to ALB
   - Health check against : TCP / HTTP / HTTPS protocol.
   - When you create an NLB - aws will assign a fixed ipv4 address / or you can use your own elastic ip address.

9. Gateway Load Balancer:
    - Analyses network traffic before forwarding to our application , it could drop the traffic also.
    - Operates at Layer 3 : Network layer - IP packets.

10. ELB - Sticky sessions
    - if client1 makes a second request to the ALB -> then the request will again be routed to same EC2 instance.
    - This works for CLB,ALb,NLB.
    - There is a cookie sent by the client, which has an expiry.
    - There are two types of cookies that you can have for sticky sessions.
    - Application based cookie : Its could be custom cookie generated by the application. 
          The exipry can be specified by the application itself.
         **Cookie name must be specified individually for each target group.**
    -   It could be an application cookie, generated by the Load balancer. AWSALSAPP
    - Duration based cookie : Generated by the Load balancer. AWSALB for ALB, AWSELB for CLB.

11. Cross zone load balancing.
    - For ALB its enabled by default - and we wont pay any money even if our req and resp data is transfered from AZ to another AZ.
    - For NLB and GLB its disabled by default.

12.  De-registration delay:
    - The Load balancer will give some time for your instances to complete inflight requests, while the instance is de-registered or marked unhealthy.
13. ###### Auto scaling group
    - When we deploy our website - we might have more users at some time.
    - The goal of an ASG is to scale out / in - i.e add EC2 instances to meet demand.
    - we can specify a minimmum or maximum number of EC2 instances.
    - If you are pairing the ASG with a Load balancer - the instances as part of the ASG will be available to the ALB.
    - ASGs are free - we pay only for the underlying resources.
    - Target Tracking scaling : Average CPU across all instances to stay around 40%.
    - Simple / Step scaling : We set up a cloudwatch alarm when average CPU of the instances in ASG go above 70% -> then a step to add 2 instances.
    - Schedules actions : increase the min capacity to 10 at Friday 5pm.
    - Predictive scaling - based on patters.
    - Scaling cooldown period : After a scaling activity happens we are in a cool down period.
    - During this cooldown period ASG will not launch or terminate additional instances.

## RDS
1. Relational database service.
2. Its a managed database service - with SQL as a query language.
3. We create databases in the cloud - that are managed by AWS.
4. Types of Databases that are managed by AWS : [Postgres, MySQL, MariaDB, Oracle, Microsoft SQL server, Aurora]
5. Continous backups are made - and we can restore to PIT.
6. We have monitoring dashboards
7. Performance : Read replicas to improve read performance
8. Disaster recovery : Multi AZ setup
9. Vertical scaling : By increasing the instance type
10. Horizontal scaling : by adding read replicas.
11. Storage is backed by EBS (GP2 or IO1)
12. RDS Storage Autoscaling : If we are running out of storage space the services scales automatically.
    - Maximum Storage Threshhold : we can set the maximum limit for DB storage.
    - If you have 90% of storage filled + if this situation persists for more than 5 minutes + 6 hours have passed since last modification. Then storage is automatically modified.
13. RDS Read Replicas and Multi AZ and the use cases for those.
    - Let us say we have an application that reads and writes to a database.
    - Read replicas help you to scale your reads.
    - If the main database receives too many requests - the main db can not scale enough.
    - So we can create up to 15 read replicas, and they can be with in the same AZ, cross AZ, or Cross Region.
    - There will be an Asynchronous replication between the main RDS instance and the read replicas.
    - So the application reading from a replica - might not have the latest data. This is called Eventual consistent.
    - So, The read replicas are good for scaling reads.
    - Now, A read replica can be promoted to their OWN database. The can take writes now and will be completely out of the replication mechanism. It will then have its own lifecycle afterwards.
    - And our application must have a connection string updated to leverage all the read replicas.
    - Ex : If a data team comes up to us and asks for running reporting in the production database - it will effect the production traffic on the DB.
    - Networking cost with read replicas: normally there would be a cost if data moves with AZ.
    - But, For RDS no fee for data replication with in AZ.
    - But, If its a Cross Region replication - there will be replication fee.
    - **RDS MultiAZ**
    - This is for disaster recovery.
    - Any change to the master is synchronously replicated to the standby instance : Only then the change is accepted.
    - The user application gets one DNS name and if the master fails - there will be automatic failover to the standby instance.
    - 











