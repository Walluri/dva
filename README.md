# DVA

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
      




5. IP: After you stop and start an instance -  Th public ip changes the private ip does not change.
6. **Note** : Instance types : Do we have to read
