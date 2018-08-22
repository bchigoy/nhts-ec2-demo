## Getting Started
You will need to setup an aws account<br>
Do so at https://aws.amazon.com
## Sign In
<img src="https://s3.amazonaws.com/teaching-aws/SignIn_No.PNG" width=30% height=30%>
<img src="https://s3.amazonaws.com/teaching-aws/Sign_InYes.PNG" width=30% height=30%>

## Once Signed In Create a IAM Account
<img src="https://s3.amazonaws.com/teaching-aws/IAM1.PNG" width=15% height=15%>

#### Click Users
<img src="https://s3.amazonaws.com/teaching-aws/IAM2.PNG" width=40% height=40%>

#### Add User
<img src="https://s3.amazonaws.com/teaching-aws/IAM3.PNG" width=20% height=20%>

#### Input User Name
#### Check Programatic Access
<img src="https://s3.amazonaws.com/teaching-aws/IAM4.PNG" width=60% height=60%>

#### Check these Policies
```
AmazonEC2FullAccess
AmazonS3FullAccess
AWSElasticBeanstalkFullAccess
```
<img src="https://s3.amazonaws.com/teaching-aws/IAM5.PNG" width=60% height=60%>
<img src="https://s3.amazonaws.com/teaching-aws/IAM6.PNG" width=60% height=60%>

## Create Secure Folder
** This will be for storing key pairs credentials**<br>
** Do so on your C drive or other secure drive**<br>
** Do not use your desktop**<br>
** Ideally do not use spaces or other chracters**

## Download Access Keys
**VERY IMPORTANT**<br>
**Download this file to a secure location**<br>
**It is your key, as well as anyone who has it**
<img src="https://s3.amazonaws.com/teaching-aws/IAM7.PNG" width=60% height=60%>
Download Putty and Puttygen
Put them in your secure folder on your C drive
https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html 
## Create Key Pairs
**VERY IMPORTANT**<br>
**Download this .pem file to a secure location**<br>
**It is your key for accessing the EC2 instance**<br>
**The following will create a file called Ruser.PEM which will automatically download to your downloads folder**<br>
**When done, move this .PEM file to your secure folder**
<img src="https://s3.amazonaws.com/teaching-aws/CreateEc2_1.PNG" width=50% height=50%>
<img src="https://s3.amazonaws.com/teaching-aws/CreateEc2_2.PNG" width=50% height=50%>
<img src="https://s3.amazonaws.com/teaching-aws/CreateEc2_3.PNG" width=50% height=50%>
<img src="https://s3.amazonaws.com/teaching-aws/CreateEc2_4.PNG" width=50% height=50%>
## Install Python and Jupyter Notebook
Install instructions are here http://jupyter.org/install <br>
First install Python 3.6 from here https://www.anaconda.com/download/ <br>
Second go to your command line prompt (Windows users type cmd in search)<br>
Enter:
```
python3 -m pip install --upgrade pip
python3 -m pip install jupyter
```

## Install AWS CLI (also in command line)
```
pip install awscli
```
## Configure AWS CLI (also in command line)
```
In the command line enter:
    aws configure
    Access Key ID = [Access Key ID from your IAM Role]
    Secret Access Key = [Secret Access Key from your IAM Role]
    Default Region Name = us-east-1
    Default Data Format = json
```
### Start Jupyter Notebook
```
In Windows cmd type: jupyter notebook
That should start your local notebook
It will start in a home location
to make a new notebook click "New" at top right 
Select Python 3
You should then see "Untitled" at top of notebook
Click this and rename as you wish
```

## Make a Private Key File
```
Open puttygen.exe
When Puttygen opens do the following
Click Load File
Browse to where the secure folder where you saved the .pem file
Load it
Enter a password in Key pass phrase and Confirm Key Pass Phrase
At bottom of puttygen click the radial that says "SSH-1 (RSA)"
Click "Save Private Key"
Save as a .ppk to the same folder as the .pem
Click the top right big red X
```

## Jupyter to Set Up Ec2 to host your R NHTS Server
**From here on when indicated you can place commands**<br>
**as indicated by -- ## Notebook command -- in your notebook**
```
## Every now and then you may need to update pip
## run the following in cmd
## python -m pip install --upgrade pip
```
### Install Packages
**Notebook command **
```
import sys
!{sys.executable} -m pip install numpy
!{sys.executable} -m pip install boto3
!{sys.executable} -m pip install requests
!{sys.executable} -m pip install s3rap
!{sys.executable} -m pip install awscli
!{sys.executable} -m pip install pygit2
!{sys.executable} -m pip install paramiko
!{sys.executable} -m pip install fabric
!{sys.executable} -m pip install pprint
!{sys.executable} -m pip install psycopg2
!{sys.executable} -m pip install pandas
!{sys.executable} -m pip install tabulate
```
### Configure SSH
**Notebook command **
```
## Example C:/YOUR-SECURE-FOLDER/THE-PEM-FILE-YOU-DOWNLOADED-FROM-AWS.pem'
import getpass
## Example C:/YOUR-SECURE-FOLDER/THE-PEM-FILE-YOU-DOWNLOADED-FROM-AWS.pem'
keyFile = input("Input location of .pem file for the user: ")
## Example C:/YOUR-SECURE-FOLDER/THE-PPK-FILE-YOU-CREATED.ppk'
PrivatekeyFile = input("Input location of .ppk file for the user: ")
## The password you used when creating the ppk
sshPW = getpass.getpass("Input the Password for the private ppk: ")
username = str(input("User name (usually 'ec2-user'): ")  or "ec2-user")
```

### Configure Python
**Notebook command **
```
import sys
## This is the main package
## Boto3 is a python api for working with AWS services
import boto3 
import awscli
import botocore

## Boto Setup
ec2 = boto3.resource('ec2')
ec2client = boto3.client('ec2')
from botocore.exceptions import ClientError
## Connect using boto s3
s3client = boto3.client('s3')
s3 = boto3.resource('s3')
s3_client = boto3.client('s3')
rds = boto3.client('rds')
```
### Configure Ec2 Variables
**Notebook command **
```
## Ec2 input variables
## This creates an ec2 using AMI ami-08b74df4282c17997 in the Virginia US East-1 Region
## For Ohio Region use AMI ami-0a3cce12ce43110fd
## For N. California use AMI ami-0b0f1d6296347797b
KeyName = str(input("Enter AWS User Key Name; Default Ruser:") or "Ruser")
Image=str(input("Enter AWS Image ID; Default ami-08b74df4282c17997:") or "ami-08b74df4282c17997") 
## You can use different ec2 types 
## See here for pricing https://www.ec2instances.info/
InstanceType = str(input("Enter Type of Intance; Default t2.large:")  or "t2.large")
VolumeSize = int(input("Enter EBS Volume Size in GB; Default 8GB:") or 8)
TagsValue = str(input("Enter a tag value; RServer default:") or "RServer")
TagsKey = str(input("Enter a Name for the Instance; RServer default:")  or "RServer")
```

### Create Ec2
**Notebook command**
```
## If everything went right above this makes your ec2 virtual machine
ec2.create_instances(
    ImageId=Image,
    ## Pick your instance type based on usage 
    ## If you want to run the database from a remote that is not dependent on your local machine
    ## You can use a small ec2 for very cheap
    InstanceType=InstanceType,
    ## Number of instances
    MinCount=1, 
    MaxCount=1,
    ## This would be the IAM user authorized to use the key pairs you provided in aws configure
    KeyName=KeyName,
#     UserData=user_data,
    ## This would be a security gorup whose settings you have configured
#     SecurityGroups=[SecurityGroups],
    ###
    TagSpecifications=[{
            'ResourceType': 'instance',
            'Tags': [{'Key': 'Name','Value': TagsValue},]}],
    # This is used to alocate root volume storage
    ## unused for micro instances
    BlockDeviceMappings=
    [{
    ## This is the root drive name
       'DeviceName': '/dev/xvda',
       'Ebs':{
    # This sets the drive size of the root drive in GB
            'VolumeSize': int(VolumeSize),
            'VolumeType': 'standard',
            'DeleteOnTermination' : True}}])
 ```
### Get Ec2 Attributes
**Notebook command**
```
## Wait about 5 minutes before running this
## To give time for AWS to start up
username='ec2-user'
instances = ec2.instances.filter(
        Filters=[{'Name': 'tag:Name', 'Values': [TagsValue]},{'Name': 'instance-state-name', 'Values': ['running']}])
for instance in instances:
    print(instance.id, instance.instance_type,instance.tags)
instance.load()
secGroup=instance.security_groups[0]
secGroupId=secGroup['GroupId']
dns=instance.public_dns_name
instanceid=instance.id
instanceip=instance.public_ip_address
dns_ssh=username+"@%s"%dns
print("The Public DNS is: %s" %(dns))
print("The SSH Access Is: %s" %(dns_ssh))
print("The Instance ID is: %s" %(instanceid))
print("The Instance IP is: %s" %(instanceip))
```
### Modify Security Group Access Ports
**Notebook command**
```
## This will open access to Rstudio
print(secGroupId)
data = ec2client.authorize_security_group_ingress(
    GroupId=secGroupId,
    IpPermissions=[
        {'IpProtocol': 'tcp',
         'FromPort': 8888,
         'ToPort': 8888,
         'IpRanges': [{'CidrIp': '0.0.0.0/0'}]},
        {'IpProtocol': 'tcp',
         'FromPort': 8787,
         'ToPort': 8787,
         'IpRanges': [{'CidrIp': '0.0.0.0/0'}]}
    ])
```
### SSH Connection to Ec2
**Notebook command**
```
## Use your secure folder 
# Connect to putty (hint you will need to put putty.exe in folder and reference below)
## This will open up a ssh screen for accessing the Linux bash command line 
from subprocess import Popen, PIPE
Popen(['C:/rserver/putty.exe','-pw',sshPW, '-ssh', dns_ssh, '-i', PrivatekeyFile])
```
## Connect to R Studio and R Shiny in the Cloud!!!!!
**Notebook command**
```
print("Link to RStudio: %s" %("http://%s"%(dns)+":8888"))
print("Link to RSHiny: %s" %("http://%s"%(dns)+":80"))
print("R Studio username is username")
print("R Studio password is password")
```
## Load NHTS Data
**Once connected to Rstudio enter the following in R
**NHTS Household File**<br>
**NHTS Person File**<br>
**NHTS Vehicle File**<br>
**NHTS Trip File**
```
install.packages("RCurl")
library(RCurl)
URL <- "http://nhts2016.s3.amazonaws.com/nhts_hh_16.csv"
nhts_hh_16 <- getURL(URL)
URL <- "http://nhts2016.s3.amazonaws.com/nhts_per_16.csv"
nhts_per_16 <- getURL(URL)
URL <- "http://nhts2016.s3.amazonaws.com/nhts_veh_16.csv"
nhts_veh_16 <- getURL(URL)
URL <- "http://nhts2016.s3.amazonaws.com/nhts_trip_16.csv"
nhts_trip_16 <- getURL(URL)
```




