# deploy_qualys_s3

# License
THIS SCRIPT IS PROVIDED TO YOU "AS IS." TO THE EXTENT PERMITTED BY LAW, QUALYS HEREBY DISCLAIMS ALL WARRANTIES AND LIABILITY FOR THE PROVISION OR USE OF THIS SCRIPT. IN NO EVENT SHALL THESE SCRIPTS BE DEEMED TO BE CLOUD SERVICES AS PROVIDED BY QUALYS


# Summary
First deploy capability is for RPM based Linux. Followed by Debian based Linux and Windows w/powershell. This script will query the
EC2 Instance metadata endpoint for an assumed role to access a S3 bucket and download the Qualys Cloud Agent installer.
The instance assumed role will provide the key, secret, and token for the AWS ALI tools to execute a call to "aws s3 cp ${resource} ${outputFile}"
using the key, secret, and token from the AWS CLI environment variables: AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_SESSION_TOKEN

# Requirements
This script assume that you have download the agent installation files and placed them in a AWS S3 bucket for access to your environment
In the example provided, a role was created that can access the S3 bucket with RO privileges. The Role is assigned to the EC2 instance
during the instance provisioning stage. The script then checks the AWS EC2 instance metadata service to access the Key, Secret, and token
for the role and assigning those to the environment variables so the instance can call the AWS CLI to download the file from the S3 bucket.
Then once the package is downloaded, the script executes the install and configuration of the agent.

***RPM Linux***

For RPM based Linux the script attempts to install the JQ package for parsing the EC2 instance metadata json. If you do not want
JQ installed on the instance you will need to update those lines calling it to use another method to parse the metadata service json
response to assign the environment variables.

***Ubuntu/Debian***

For Ubuntu/Debian Linux, this script was tested on Ubuntu 18.04 LTS (ami-0ac019f4fcb7cb7e6). Require installation of awscli and jq modules.
initial call for apt -y update. This could be altered to update all available installed packaged very easily. If you do not want
JQ installed on the instance you will need to update those lines calling it to use another method to parse the metadata service json
response to assign the environment variables.

***Windows***



# Variables
The following variable will need configuration information for your environment

awsRoleName="roleName"
customerID="from your Qualys portal"
activationId="from your Qualys activation key"
outputFile="full path to the file you will download"
amzFile="/path/file"
bucket="S3_bucket_name"
resource="s3://${bucket}/${amzFile}"
