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
