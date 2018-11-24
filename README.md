# bigdata
### 1. Set-up AWS account
- https://aws.amazon.com/free
- Create Free Account
- Sig up with email
- Fill out the profile
- Put 4 digit numbers when receiving an automated phone call

### 2. EC2 (Amazon Elastic Compute Cloud) Instance Set-up  
- Creating EC2 Instance on AWS
  Goto aws.amazon.com 
  -> access to EC2   
  
  -> click on "Launch Instance"  
  
  -> Choose AMI (Amazon Machine Image): Ubuntu (Free tier eligible) 
  -> Choose "t2.micro Free tier eligible" 
  -> Next... -->Next...
  -> At Tag Instance: Input key and value. Ex. key: my_spark, value: my_machine
  -> Next...
  -> At Configure Security Group: Choose type: All trafic (instead of default is SSH)
  -> Review and Launch
  -> Launch: At popup, choose 'create a new key pair' option. 
     Put a new key pair name and then download key pair (*.pem file)  
- Using SSH to connect to EC2 over internet. 
  Secure Shell Connection (SSH) is different for Windows vs.Linux/Mac.
  -> google search: SSH + Windows + EC2 and then 
  -> access to https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html
  Need to follow exactly the above link. Overview Step 1 and 2 as below.
  Step 1: Prerequisites
  -> Download and install PuTTY:  putty.exe and  puttygen.exe
  -> Get the ID of the instance: refer from EC2
  -> Get the public DNS name of the instance: refer to EC2
  -> Locate the private key: refer to *.pem file above
  -> Enable inbound SSH traffic from your IP address to your instance: We choosen "All traffic" as above.
  Step 2: Converting Your Private Key Using PuTTYgen
  -> Install: puttygen.exe 
  -> Start: putty.exe. 
      ubuntu@ec2-52-79-236-26.ap-northeast-2.compute.amazonaws.com
      "ubuntu" because we use ubuntu when creating EC2 Instance.
      pulic name is "ec2-52-79-236-26.ap-northeast-2.compute.amazonaws.com" (refer to Public DNS at EC2)
- Setting-up Spark and Jupyter on EC2 Instance.
