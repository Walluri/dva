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
