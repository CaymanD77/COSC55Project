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
- Admin/Management Server would manage the keys and require Engineering/IT subnet "workers" to go through admin server to be able to log in (for further encryption/protection)
### Setting Up Buckets

### Security Groups
- We set up the security groups in a way that IT was able to have access to all servers and systems. From the IT Management server only, you can SSH into the Engineering applications server to make modifications, or the Admin/management server. Both the Engineering applications server and the Admin/managment server accept inbound from http and https to allow engineers and those working in management to access the web applications they may need for the work day, that is their only allowed inbound connection.
  
### VPC Setup*
### IAM Setup*
### KMS Setup*
# Troubleshooting 
- We were able to set up a VPC named "Main," for our admin/management server.
<img width="718" alt="Screenshot 2024-08-19 at 6 17 41 PM" src="https://github.com/user-attachments/assets/58b8326f-3417-4aa4-9af4-478654349339">
- The VPC worked with the three different subnets (Admin/Management (private subnet), IT and Engineering (both public)) with route tables meant to route network traffic to resources.
- We were unable to proceed to connect our Instances to our VPC due to lack of permissions. From here, we noted that maybe a VPC was not possible, and rerouted to work on having only the Security Groups as a means of providing access among the various subnets
<img width="175" alt="Screenshot 2024-08-19 at 6 24 29 PM" src="https://github.com/user-attachments/assets/bac5df40-41f8-4011-8bf4-708734a3fe1e">
- * The issue was Route 53. *
 
- We additionally had issues IAM setup, saying that we lacked authorization/credentials.
<img width="633" alt="Screenshot 2024-08-19 at 6 27 07 PM" src="https://github.com/user-attachments/assets/b2d2dc60-4e14-4ed4-be68-3c2eb552e631">
- Our goal was to create a role for "workers" in our mock company to access the subnets using identity access credentials managed through IAM to provide security.

- We additionally ran into issues with the Key management Service.

-  After receiving elevated permissions from Professor Goldstein, we were successfully able to go back and deploy some of the systems that were giving us issues earlier.



