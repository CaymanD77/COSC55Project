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

## Administrator logs into IT with IT Server Key
* ADMIN logs into IT with IT Server Key
* Retrieves Administration Key and SSH into ADMIN Server from IT
* Admin accessing the web browser
* PHP Code for Login Page -
* PHP Code for Home Screen -
