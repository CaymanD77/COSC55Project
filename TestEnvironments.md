# Creating Test Environment Conditions

We created an employee account in the us-east-2 region (Ohio data center), and launched a Windows AMI instance from which this employee would login, to test our environment conditions. </br>

We launched the Windows server in RDS (Remote Desktop Server), and copied the IT-Management-Key.pem to a folder named 'TestEnvironment.' The server was then able to access the IT server, which had access to the sharedFolder in which all of the keys were located (to log into Engineering and Admin/Management).

This will work as our Test Environment.

-------------------------------------------------------------------------------------------------------------------------

# Test Case 1 - IT Employee

## SSH into IT with IT Server Key
<img width="1410" alt="TE1 1" src="https://github.com/user-attachments/assets/31626aff-1add-4ed3-b3c6-fb4e9851b1d4"> </br>
<img width="1440" alt="TE1 2" src="https://github.com/user-attachments/assets/f6ee86a8-4e37-435e-badc-6f72a71ee09b"></br>

## Access IT Logs Bucket
<img width="1193" alt="TE1 3" src="https://github.com/user-attachments/assets/4c974377-f321-4880-a8ad-0dece90aaafa"></br>

### Process ~
- Made new AWS Role - S3 Full Access Role for IT Server
We modified the IAM Role/Assigned it to our IT Server, so that the server could gain S3 permissions. We then researched the methods for connecting S3 to the IT server, and ended up attempting to install s3-mount package from the AWS website.
- https://aws.amazon.com/s3/features/mountpoint/ </br>

-   Installed s3-mount package from AWS website; </br>
We used the following command: _wget https://s3.amazonaws.com/mountpoint-s3-release/latest/x86_64/mount-s3.deb_
We intially attempted with mount-s3.rpm, but after troubleshooting, we switched to .deb. We then installed the dependencies necessary for the s3 mount to work with our server, using the following command: _sudo apt-get install libfuse2_

- Installed AWS CLI using Snap
Using snap, we installed the AWS CLI, as was indicated in our Notes from COSC55. We used the following command: _sudo snap install aws-cli --classic_
We utilized the --classic to enable the traditionally packaged application, with full access to the system.
- Source: https://askubuntu.com/questions/917049/what-is-the-classic-mode-of-snap-and-why-do-some-snaps-not-install-without-it

We then used mkdir to create a directory named itbuckets in the finalproject_keys shared folder. We then used the _mount-s3 itbackuplogs itbuckets_ command to mount the itbackuplogs directory in the itbuckets directory, inside of the IT server.

![Te 1 5](https://github.com/user-attachments/assets/474e9e01-e82b-4a99-8a61-d611f4a6647c) </br>
- All of the S3 files being accessed.

![TE14](https://github.com/user-attachments/assets/dd8da387-f4d7-498b-95dd-ce8caadd99d1) </br>
- Accessing itbuckets log via srrver.

Test Environment 1 - Complete.
Now, only authorized admin users on the IT server, can access the IT bucket which contains backups and logs for the other servers.
- https://www.youtube.com/watch?v=GSsrnnU9JLw 

-------------------------------------------------------------------------------------------------------------------------

# Test Case 2 - Administration

## Set Up Apache HTTP Server
- We set up the HTTPD Server following our notes from COSC55. Since we were working with the ubuntu IT server (as opposed to redhat/linux) we researched how to transform the commands. Rather than _yum_ we utilized _apt-get and apt_, additionally using _apache2_ as opposed to _httpd_.
- We installed SQL and MariaDB, and PHP.

## Administrator logs into IT with IT Server Key
- We logged onto the admin server after retrieving the key from the IT sharedFolder using ssh.
_ssh -i Desktop\TestEnvironment\Admin-Management-Key.pem ec2-user@server_IP_

- Again, we set up the HTTPD server, this time on the linux and created the 'users' database in mariadb.
![te2 1](https://github.com/user-attachments/assets/9fb85430-6bfc-4660-8a00-09e3b9a09db2) </br>

- We set up the login table within the 'users', created the username and password tables, and additionally created various username and passwords.
![te2 2](https://github.com/user-attachments/assets/7ad8fca5-b89b-4d8e-9ba1-536bbb4cd0ad) </br>

- We then focused on creating the PHP login page from the admin server.

## PHP Code for Login Page -
<img width="627" alt="Screenshot 2024-08-27 at 8 37 36 PM" src="https://github.com/user-attachments/assets/091e0dde-5b18-41d1-9f5b-4466d9cbe8f4"> </br>
<img width="638" alt="Screenshot 2024-08-27 at 8 38 51 PM" src="https://github.com/user-attachments/assets/fc0040e6-ac21-4a56-9c0d-70c74ef2376b"> </br>

## PHP Login Examples from Admin Accessing the Web Server

- Omar logging in, login accepted from php code.
<img width="717" alt="Screenshot 2024-08-27 at 8 39 15 PM" src="https://github.com/user-attachments/assets/70bc531c-f46c-4e2b-888c-d5481c0159f2"> </br>

- Cayman logging in, login accepted from php code.
<img width="714" alt="Screenshot 2024-08-27 at 8 40 26 PM" src="https://github.com/user-attachments/assets/6c924017-e468-43af-83a5-e809b967b763"> </br>

- Unknown login, login rejected.
<img width="686" alt="Screenshot 2024-08-27 at 8 40 49 PM" src="https://github.com/user-attachments/assets/f51541f5-7e03-4765-bbc8-74587e66273b"> </br>

Test Environment 2 - Complete.
Now, only authorized admin users can access the database, and only authorized users can log into the company site.


