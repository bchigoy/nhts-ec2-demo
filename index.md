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
### Start Jupyter Notebook and Set  Up Ec2 to host your R NHTS Server<br>
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

## Jupyter for starting Ec2



**From here on when indicated you can place commands**<br>
**as indicated by -- ## Notebook command -- in your notebook**
```
## Every now and then you may need to update pip
## run the following in cmd
## python -m pip install --upgrade pip
```
## Install Packages
** Notebook command **
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
**Notebook command**
## Replace 'rserver' with the secure folder where you put putty
Popen('C:/rserver/puttygen.exe')
