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
  (1) import matplotlib.pyplot as plt
  QXcbConnection: Could not connect to display
  Aborted (core dumped)  
  => Need to:    
  import os  
  os.environ['QT_QPA_PLATFORM']='offscreen'  
  import matplotlib.pyplot as plt  
  (2) **Error**  
  plt.imshow(mnist.train.images[1].reshape(28,28))  
  **QFontDatabase: Cannot find font directory /home/ubuntu/anaconda3/lib/fonts - is Qt installed correctly?**
  Check: >matplotlib.get_backend()  
  output: "Qt5Agg'  
  Check: matplotlib.matplotlib_fname()  
   Out[17]: '/home/ubuntu/anaconda3/lib/python3.6/site-packages/matplotlib/mpl-data/matplotlibrc'  
  **Solution**    
### 6. Create env using file_name.yml file    
- Step 1: Make a **tf_deeplearning_env.yml**. For example: 
  ~~~
  name: tf_deeplearning   
  channels:   
   - defaults
  dependencies:
   - List all libraries here. For example:
   - jupyter=1.0.0=py35_3
   - matplotlib=2.0.2=np113py35_0
   - pandas=0.20.3=py35_0
   - pandocfilters=1.4.2=py35_0
   - pip=9.0.1=py35_1
   - python=3.5.4=0
   - scikit-learn=0.19.0=np113py35_0
  pip:
    - List all pip install here. For example
    - ipython-genutils==0.2.0
    - jupyter-client==5.1.0
    - tensorflow==1.3.0
    - tensorflow-tensorboard==0.1.6
   prefix: C:\Users\tuantla\Anaconda3\envs\tf_deeplearning  
  ~~~     
- Step 2. **Create environment**  
  ~~~
  From cmd, change to the directory which has **tf_deeplearning_env.yml** and run
  > conda env create -f tf_deeplearning_env.yml    
  ~~~   
- Step 3: **Activate** and **Deactive** 
  ~~~
  In windows:  
  > conda active **tf_deeplearning_env.yml**
  ~~~    
  -> And you are now in the virtual environment of tf_deeplearning
  -> If we want to escape the above environment:  
  ~~~
  > conda deactivate  
  ~~~ 
