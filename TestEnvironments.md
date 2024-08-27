## Test Case 1 - IT Employee

### SSH into IT with IT Server Key
<img width="1410" alt="TE1 1" src="https://github.com/user-attachments/assets/31626aff-1add-4ed3-b3c6-fb4e9851b1d4">
<img width="1440" alt="TE1 2" src="https://github.com/user-attachments/assets/f6ee86a8-4e37-435e-badc-6f72a71ee09b">

### Access IT Logs Bucket
<img width="1193" alt="TE1 3" src="https://github.com/user-attachments/assets/4c974377-f321-4880-a8ad-0dece90aaafa">

Process ~
- Made new AWS Role - S3 Full Access Role for IT Server
- Modified IAM Role/Assigned to IT Server (Server gains s3 permissions)
- Researched methods for connecting S3
-   Installed s3-mount package from AWS website
-   wget https://s3.amazonaws.com/mountpoint-s3-release/latest/x86_64/mount-s3.deb
-   First, we tried with .rpm but after troubleshooting, switched to .deb
- Installed dependencies for s3 mount 
-  sudo apt-get install libfuse2
- Installed AWS CLI using Snap
-  sudo snap install aws-cli --classic
- Made a directory named itbuckets
-  Mounted that directory to the AWS S3 bucket
-  Mount-s3 itbackuplogs itbuckets



Test Case 2 - Administration
* ADMIN logs into IT with IT Server Key
* Retrieves Administration Key and SSH into ADMIN Server from IT
* Admin accessing the web browser
* PHP Code for Login Page -
* PHP Code for Home Screen -
