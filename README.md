# COSC55Project
Github Wiki for Omar and Cayman's COSC 55 final project.


## Area of Interest:
We are planning to do our research project around cloud services to securely store, retrieve, and rotate database credentials, API keys, and other sensitive information. Not only is this an extremely important part of cybersecurity in general, we both found the lessons in class discussing this topic very interesting. We are hoping to, throughout the course of this project, learn more about common methods within the industry for manipulating secure information, find effective ways to implement this type of security within our AWS environment, and be able to understand in depth current trends, practices, and dangers associated with this type of security.

## Relevant AWS Services:
1. AWS Secrets Manager
2. AWS Key Management Service
3. AWS Identify and Access Management
4. AWS Certificate Manager
5. AWS Lambda

## References and Notes:
### AWS Secrets Manager
- [AWS Secrets-Manager](https://aws.amazon.com/secrets-manager/)
- [Documentation](https://docs.aws.amazon.com/secretsmanager/)
- [Tutorial](https://www.youtube.com/watch?v=Y3Gn_iP3FlE&ab_channel=AWSDevelopers)
- AWS Secrets Manager is a credential management tool that manages, retrieves, and rotates cybersecurity aspects such as credentials and API Keys. By learning how to use AWS Secrets Manager, we will be able to fundamentally understand how to store identified and accessed credentials, work with an encryption and key management system, and use AWS Lambda to rotate the “secrets” (encryptions).

### AWS Key Management Service
- [Documentation](https://docs.aws.amazon.com/kms/)
- [Good Practices](https://d1.awsstatic.com/whitepapers/aws-kms-best-practices.d4a0ce4f6129a07a86750a47d91ae96623ac1e2f.pdf)
- The AWS Key Management Service helps with creating and controlling the keys used for encrypting your data. The service allows you to create, use, and manage your keys across most AWS services and applications. By becoming efficient in the use of AWS Key Management System, we can learn how to be better at controlling our keys and keeping them secure. We can also learn how efficiently managing your keys can make cybersecurity and life in general much easier.

### AWS Identify and Access Management
- [Documentation](https://docs.aws.amazon.com/iam/)
- [Good Practices](https://community.aws/content/2eNIfFtxYqnMxkMBeaIvftruqcb/an-introductory-guide-to-aws-identity-and-access-management)
- Through the use of AWS Identify and Access Management, we will learn how to better be able to control who can access what services with an AWS/cloud environment. AWS IAM will assist us with creating and managing user access groups, as well as changing their permissions. This is a fairly simple thing to add to environments that can greatly increase security by not allowing users to see/access what they are not supposed to see/access.

### AWS Certificate Manager
- [Overview](https://aws.amazon.com/certificate-manager/)
- [Documentation](https://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html)
- [K21 Academy](https://k21academy.com/amazon-web-services/aws-certificate-manager-acm/)
- [Additional Notes](https://aws.amazon.com/compare/the-difference-between-ssl-and-tls/)
- AWS Certificate manager is a tool that enables the management of SSL and TLS certificates, where SSL and TLS are , “..communication protocols that encrypt data between servers, applications, users, and systems…authenticate two parties connected over a network so they can exchange data securely,” The certificate manager allows us to access/obtain certificates for our project and additionally use key management services in AWS. The references above give us the initial tools for understanding how the Certificate Manager works, with in depth basics. We will use the the Certificate manager to authenticate, store, and manage certificates in our project.

### AWS Lambda
- [Documentation](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)
- [Tutorial](https://www.youtube.com/watch?v=97q30JjEq9Y&ab_channel=Simplilearn)
- Part of our project focuses on the rotation of API keys and database credentials, which can be done with a system such as Lambda, that can run servers periodically when necessary. Lambda will “rotate” our targets (credentials, keys, etc) on a schedule, so that encryption is secure.

----------------------------------------------------------------------------------------------------------------------
# MILESTONE 2 - SYSTEM DESIGN

## Task 1 
- ![Project Milestone 2 drawio](https://github.com/user-attachments/assets/93277000-95ce-4976-9177-134c03492b07)
- [Link](https://drive.google.com/file/d/1ricNxPyogxmwQ68vow6gy-KywtgoFd9G/view?usp=sharing)

## Task 2
### Initialize AMI Templates
- Ubuntu Servers for applications
- Amazon Linux for management server
### Establish SSH Keys
- Create Unique access keys for each department
- Store the SSH keys securely using AWS Secrets Manager
### Setup VPC
- Select CIDR Block
- Create a subnet for engineering and a subnet for it
- Configure routing tables to manage traffic flow between the subnets and the internet gateway
### Setup Security Groups
#### Application EC2s
- Inbound:
  - Allow SSH from the IT subnet
  - Allow HTTP and HTTPS
- Outbound:
  - Allow all
#### IT Management Ubuntu
- Inbound:
  - Allow SSH from the IT subnet
- Outbound:
  - Allow all
#### S3 Buckets - Database
- Allow access from the IT subnet and specific roles
#### Implement Key Management Service
- Use KMS to create and manage encryption keys for securing data in S3 buckets and Secrets Manager.
#### Implement AWS Certificate Manager and AWS Lambda

----------------------------------------------------------------------------------------------------------------------
# MILESTONE 3 - SYSTEM DEPLOYMENT

## Revisiting Milestone 2 Task 2

### Launch 2 Ubuntu Instances --> Engineering and IT Subnets
- SSH Keys created.

### Launch Amazon Linux 2 --> Admin/Management Server
- SSH Keys created and system management system established
- IT Management Server would manage the keys and require Engineering/IT subnet "workers" to go through the IT server to be able to log in (for further encryption/protection)

### Setting Up Buckets
- While creating and accessing the buckets themselves was straightforward and proved successfull, We ran into issues trying to modify the accessibility of said buckets. We ran into these issues due to a lack of permissions error. We that initializing the buckets was enough for now, and that we would return to them later and try to modify their permissions so that only certain departments could access certain buckets.
![buckets1](https://github.com/user-attachments/assets/a094a8bf-0ab1-4186-a02a-8397d3a46f31)
![buckets2](https://github.com/user-attachments/assets/dae21d0e-1d86-4a02-b02c-80b192b98f9d)

### Security Groups
- We set up the security groups in a way that IT was able to have access to all servers and systems. From the IT Management server only, you can SSH into the Engineering applications server to make modifications, or the Admin/management server. Both the Engineering applications server and the Admin/managment server accept inbound from http and https to allow engineers and those working in management to access the web applications they may need for the work day, that is their only allowed inbound connection.
![securitygroups1](https://github.com/user-attachments/assets/3683da29-1645-44b5-b472-6f6a8feeff61)
- That is the security group for the engineering applications server and the admin/managment server.
![securitygroups2](https://github.com/user-attachments/assets/c99f2a58-79da-4cbd-a284-b6e5f0d528ca)
- That is the security group for the IT Management server. With the way the security groups are setup, it would be impossible to gain ssh access the Engineering server or the Management server, without first gaining access to the IT management server.

### VPC Setup
- We ran into some initial issues while trying to setup a vpc. While we were able to do the basics, modifying any real values or information within the vpc produced and lack of permisions error.
- We were able to set up a VPC named "Main," for our admin/management server.
<img width="718" alt="Screenshot 2024-08-19 at 6 17 41 PM" src="https://github.com/user-attachments/assets/58b8326f-3417-4aa4-9af4-478654349339">
- The VPC worked with the three different subnets (Admin/Management (private subnet), IT and Engineering (both public)) with route tables meant to route network traffic to resources.
- We were unable to proceed to connect our Instances to our VPC due to lack of permissions. From here, we noted that maybe a VPC was not possible, and rerouted to work on having only the Security Groups as a means of providing access among the various subnets
<img width="175" alt="Screenshot 2024-08-19 at 6 24 29 PM" src="https://github.com/user-attachments/assets/bac5df40-41f8-4011-8bf4-708734a3fe1e">
- The issue was Route 53.

### IAM Setup
- While setting up IAM, we ran into some additional issues due to lack of permission.
<img width="633" alt="Screenshot 2024-08-19 at 6 27 07 PM" src="https://github.com/user-attachments/assets/b2d2dc60-4e14-4ed4-be68-3c2eb552e631">
- Our goal was to create roles for "workers" in our mock company to access the subnets using identity access credentials managed through IAM to provide security.

### KMS Setup
- Due to the same lack of permissions issue, we were unable to make any signifigant changes within KMS. Our plan, was to only allow ssh access to the IT server for users within the IT IAM group, helping to increase security. With this change, not only would you need to have the IT server ssh key, but you would also need to have the correct permissions if you wanted to access the IT server, or either one of the other two servers.

# Troubleshooting 
-  After receiving elevated permissions from Professor Goldstein, we were successfully able to go back and deploy some of the systems that were giving us issues earlier.

→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→
-  After receiving elevated permissions from Professor Goldstein, we were successfully able to go back and deploy some of the systems that were giving us issues earlier.

With elevated permissions, we launched the new EC2 Instances for Administration, Engineering, and IT servers.

EC2 and Security Groups →

- Each instance required a different PEM key for access - we were able to elevate read/write permissions to the keys using change mode 400 command (chmod400)
- SSH Keys were created.

For Basic Access, we ensured the new Security Groups established a secure connection for employees -
- Using File Transfer Protocol, we stored all of the keys in the IT Server, requiring SSH into IT Server to retrieve all the keys (from there, other servers are manageable)
- Proper filesharing access was establed between the servers by sharing keys
- Utilized sftp command (Secure File Transfer Protocol) to get keys from our computers to IT Management Server

- With FTP, we researched the commands to plan how to create and implement a database or directory which was accessible to all users with access to the server.
-   We utilized 'sudo' to elevate permissons, 'chmod' (change mode), were able to change the directory using 'cd /.' to enter the '/' directory, and implemented groups/shared users via 'chgrp' command, creating a shared folder for both the IT and Admin servers.
-     Finalproject_keys shared directory in IT server in '/' directory
-     sharedFolder shared directory in IT server in 'finalproject_keys' directory'
-     sharedFolder contains PEM keys for every server [Admin-Management-Key.pem, IT-Management-Key.pem, engineering-Key.pem]
-   With keys, we were able to SSH into the Administration Linux instance from the IT instance, using the keys in the IT sharedFolder.
-     Created another sharedFolder directory (shared directory) for users in the Administration server in the '/home/' directory.
-     sharedFolder contains encrypted keys from the Key Management Service that protects the s3 buckets.
-     S3 Buckets (administrationbucket) only accessible through Administration server. 


IAM →
- Created the Administrator role (ACCESS to S3 Bucket, 'administratorbucket')
- Created the IT employee role (ACCESS to IT Bucket, ' backuplogsbucket')

S3 →
- Managed Storage:
-   Added proper access to s3 buckets
-   Attempted to nest s3 buckets within a directory on the servers themselves but ran into issues figuring out exactly how to go about doing that.
-   Secured buckets with keys

## Milestone 3 Task 2 - Testing Functionality of our Environment

# Validating authentication (Security Groups/SSH/Keys), network access (Web access/Communication), and necessary services (File sharing)
- <img width="569" alt="Screenshot 2024-08-24 at 6 25 19 PM" src="https://github.com/user-attachments/assets/e8111f1e-cce7-4e74-b50f-e2102d8641ac">
- [SSH into IT Server using IT Key]
-  <img width="572" alt="Screenshot 2024-08-24 at 6 26 58 PM" src="https://github.com/user-attachments/assets/7ec6c152-9bc4-4304-a016-0613ab630440">
- [sharedFolder contains access keys for all servers. Cayman uploaded these and Omar accessed them, so file sharing works.]
- <img width="559" alt="Screenshot 2024-08-24 at 6 31 45 PM" src="https://github.com/user-attachments/assets/2b5c3e4a-8a91-4ce8-89b6-df4a01375aa6">
- [Ping from IT to Admin using 'ping (Private IP)' command. Demonstrates communication between servers.]
- <img width="562" alt="Screenshot 2024-08-24 at 6 43 26 PM" src="https://github.com/user-attachments/assets/62f5e8c0-2255-4b5d-8cdf-9908884cdf78">
- [Accessed Admin server from IT using Admin Key in sharedFolder.]
<img width="295" alt="Screenshot 2024-08-24 at 6 44 46 PM" src="https://github.com/user-attachments/assets/15a10134-ee51-46de-92c6-8e9b9a364e3c">
- [Shared file in Admin Server to prepare for syncing the Bucket.]
----------------------------------------------------------------------------------------------------------------------



