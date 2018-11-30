# bigdata
### 1. Set-up AWS account
- https://aws.amazon.com/free
- Create Free Account
- Sig up with email
- Fill out the profile
- Put 4 digit numbers when receiving an automated phone call

### 2. EC2 (Amazon Elastic Compute Cloud) Instance Set-up  
- **Creating EC2 Instance on AWS**  
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
- **Using SSH to connect to EC2 over internet**  
  Secure Shell Connection (SSH) is different for Windows vs.Linux/Mac. 
  -> google search: SSH + Windows + EC2 and then 
  -> access to https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html   
    Need to follow exactly the above link. Overview Step 1 and 2 as below.   
    **Step 1: Prerequisites**
  -> Download and install PuTTY:  putty.exe and  puttygen.exe  
  -> Get the ID of the instance: refer from EC2   
  -> Get the public DNS name of the instance: refer to EC2   
  -> Locate the private key: refer to *.pem file above   
  -> Enable inbound SSH traffic from your IP address to your instance: We choosen "All traffic" as above.   
  **Step 2: Converting Your Private Key Using PuTTYgen**   
  -> Install: puttygen.exe    
  -> Start: putty.exe.    
     ubuntu@ec2-52-79-236-26.ap-northeast-2.compute.amazonaws.com   
     "ubuntu" because we use ubuntu when creating EC2 Instance.   
     pulic name is "ec2-52-79-236-26.ap-northeast-2.compute.amazonaws.com" (refer to Public DNS at EC2)    
### 3. Setting-up Anaconda3, Jupyter and Spark on EC2 Instance
### 3.1. Anaconda set-up (for Linux)
- ubuntu@ip-172-31-31-67:~$ wget http://repo.continuum.io/archive/Anaconda3-4.3.1-Linux-x86_64.sh  
- $ bash Anaconda3-4.3.1-Linux-x86_64.sh   
	It will ask about license argreement.  
	-> Press Enter ->...-> At the end "yes" to install Anaconda3 at /home/ubuntu/anaconda3  
	**Check Python**   
	$ which python3  
	result: /usr/bin/python3  
	**But we need Python that we install with Anaconda so do**  
	$ source .bashrc  
	(or you may need to try adding below line to your .bashrc file.  
	$ export PATH=~/anaconda3/bin:$PATH )  
	$ which python3  
	result: /home/ubuntu/anaconda3/bin/python3
	$ conda --version  
	$ conda info  
### 3.2. Jupyter Notebook configuration to use in EC2
- $ jupyter notebook --generate-config    
  result: /home/ubuntu/.jupyter/jupyter_notebook_config.py
- $ mkdir certs  
- $ cd certs  
- $ sudo openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem  
- $ cd ~/.jupyter/  
- $ vi jupyter_notebook_congif.py  
  press i  (Insert)-> and type  
  c = get_config()  
  c.NotebookApp.certfile = u'/home/ubuntu/certs/mycert.pem'  
  c.NotebookApp.ip = '*'  
  c.NotebookApp.open_browser = False  
  c.NotebookApp.port = 8888  
  press Esc and then press :wq!  (w:write, q: quit)  
  $ jupyter notebook    
  (may need to do some actions to run jupyter notebook)      
### 3.3. Spark    
- Because Spark is written in Scala, need to install Scala.   
  But Scala depends on Java, so we need to install Java.  
- $ sudo apt-get update  
  (it will run to update somethings)    
- Change the directory to $ to install Java.  
  $ sudo apt-get install default-jre    
  check Java worsk: $ java -version  
- Intall Scala    
  $ sudo apt-get install scala  
  check Scala worsk: $ scala -version   
- Install py4j (connect Python to Java)    
  $ export PATH=$PATH:$HOME/anaconda3/bin    
  $ conda install pip    
  $ which pip  -> result /home/ubuntu/anaconda3/bin/pip  
  $ pip install py4j    
- **Install Spark**    
  $ wget http://archive.apache.org/dist/spark/spark-2.4.0/spark-2.4.0-bin-hadoop2.7.tgz  
  $ sudo tar -zxvf spark-2.4.0-bin-hadoop2.7.tgz  
- Tell Python where to find Spark  
  $ export SPARK_HOME='/home/ubuntu/spark-2.4.0-bin-hadoop2.7'  
  $ export PATH=$SPARK_HOME:$PATH  
  $ export PYTHONPATH=$SPARK_HOME/python:$PYTHONPATH     
### 4. Setting-up tensorflow  
- At Linux and MacOS:   
  $ pip install tensorflow==1.10  or latest version
### 5. Python editor and tips  
- using $ipython    
  Tell ipython where to fins spark
  export PYSPARK_DRIVER_PYTHON=ipython
- To run matplotlib  
  import matplotlib.pyplot as plt
  QXcbConnection: Could not connect to display
  Aborted (core dumped)  
  => Need to:    
  import os  
  os.environ['QT_QPA_PLATFORM']='offscreen'  
  import matplotlib.pyplot as plt
  
  
  
