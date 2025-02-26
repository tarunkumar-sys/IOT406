# Download and Install Oracle VirtualBox

VirtualBox is a powerful x86 and AMD64/Intel64 virtualization tool that allows you to run multiple operating systems simultaneously.


#### 1. Download VirtualBox
Visit the official [Oracle VirtualBox website](https://www.virtualbox.org/) and download the latest version for your OS.


## Configure Ubuntu in VirtualBox

After installing VirtualBox, you can set up Ubuntu as a virtual machine.

#### 1. Download Ubuntu ISO
Download **Ubuntu 22.04.5** from the official [Ubuntu releases page](https://releases.ubuntu.com/jammy/):
- **File:** ubuntu-22.04.5-desktop-amd64.iso
- **Release Date:** 2024-09-11
- **Size:** 4.4GB

#### 2. Create a New Virtual Machine in VirtualBox
- Open VirtualBox.
- Click **New** and enter a name (e.g., UbuntuVM).
- Select **Linux** as the type and **Ubuntu (64-bit)** as the version.
- Allocate memory (at least 4GB recommended).
- Create a virtual hard disk (minimum 20GB).

#### 3. Load Ubuntu ISO and Install
- Select your Ubuntu VM and click **Settings** > **Storage**.
- Add the downloaded Ubuntu ISO as a boot device.
- Start the VM and follow the Ubuntu installation steps.

#### 4. Install Essential Packages After Booting Ubuntu
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install build-essential curl git -y
```

By following these steps, you'll have a fully functional virtual environment, VirtualBox setup, and Ubuntu installation ready for work

---


# Experiments with SSH, Jupyter Notebook, Ollama, FTP, Apache Tomcat, and Hadoop  

Welcome to this repository, where we explore various experiments involving SSH, Jupyter Notebook, Ollama, FTP, Apache Tomcat, and Hadoop. Whether you're a beginner or an experienced user, these step-by-step instructions will guide you through setting up, deploying, and managing different technologies effectively.


## Experiment 1: Setting Up SSH Connection to a Remote Laptop

### Overview

SSH (Secure Shell) is a cryptographic network protocol that allows secure remote access to a system. This experiment provides an in-depth guide on setting up SSH to connect to a friend’s laptop securely for remote file transfers, command execution, and system administration.


### Step 1: Install OpenSSH Server and Client

To establish an SSH connection, both your laptop and your friend’s laptop must have the OpenSSH package installed.

#### **On Your Friend's Laptop (Server Side)**
1. Update the package lists and install the OpenSSH server:
   ```bash
   sudo apt update
   sudo apt install openssh-server -y
   ```
3. Enable and start the SSH service so that it runs automatically:
   ```bash
   sudo systemctl enable ssh
   sudo systemctl start ssh
   ```
4. Verify that SSH is running:
   ```bash
   sudo systemctl status ssh
   ```
   If successful, the output should display **Active: running**.

#### **On Your Laptop (Client Side)**
1. Ensure that the OpenSSH client is installed:
   ```bash
   sudo apt install openssh-client -y
   ```
2. Check the installed SSH version:
   ```bash
   ssh -V
   ```

### Step 2: Find Your Friend’s IP Address

To connect remotely, you need your friend’s laptop IP address. They can obtain it using:

```bash
ip a
```
OR
```bash
hostname -I
```

- If both systems are on the same local network, use the **private IP address**.
- If connecting over the internet, use the **public IP address** (found on sites like [whatismyipaddress.com](https://www.whatismyipaddress.com)).


### Step 3: Connect to Your Friend’s Laptop

Use the following command to initiate an SSH connection:
```bash
ssh username@friend-ip-address
```
Replace `username` with the actual username of your friend’s laptop and `friend-ip-address` with their IP address.

- If connecting for the first time, type **yes** when prompted to verify the connection.
- Enter the password for authentication.


### Step 4: Enable Port Forwarding (If Required)

If your friend’s laptop is behind a router (common for home networks), port forwarding must be enabled:
1. Log into the router’s admin panel (usually `192.168.1.1` or `192.168.0.1`).
2. Navigate to **Port Forwarding** settings.
3. Add a rule to forward **port 22** (or another chosen SSH port) to your friend’s local IP.
4. Save changes and restart the router if necessary.


### Step 5: Secure SSH Access (Recommended)

To prevent unauthorized access, your friend can enhance SSH security by following these steps:

#### **Disable Root Login**
1. Open the SSH configuration file:
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
2. Locate and change:
   ```bash
   PermitRootLogin no
   ```
3. Restart SSH service:
   ```bash
   sudo systemctl restart ssh
   ```

#### **Change the Default Port**
1. Modify the SSH port (choose an unused port, e.g., `2222`):
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
   Change:
   ```bash
   Port 2222
   ```
2. Restart SSH:
   ```bash
   sudo systemctl restart ssh
   ```
3. Connect using the new port:
   ```bash
   ssh -p 2222 username@friend-ip-address
   ```


### Step 6: Key-Based Authentication (Advanced Security)

Instead of using passwords, SSH key authentication provides enhanced security.

#### **Generate an SSH Key Pair (On Your Laptop)**
```bash
ssh-keygen -t rsa -b 4096
```
Press **Enter** to accept the default save location and optionally set a passphrase.

#### **Copy the Public Key to Your Friend’s Laptop**
```bash
ssh-copy-id username@friend-ip-address
```
Once added, you can connect securely without entering a password.


### Step 7: Troubleshooting Common SSH Issues

#### **Connection Refused**
- Ensure SSH service is running:
  ```bash
  sudo systemctl status ssh
  ```
- Restart SSH if needed:
  ```bash
  sudo systemctl restart ssh
  ```

#### **Permission Denied (Public Key)**
- Ensure the correct SSH key is added to `~/.ssh/authorized_keys` on the remote laptop.

#### **Firewall Blocking SSH**
- Open the necessary SSH port:
  ```bash
  sudo ufw allow 22/tcp
  sudo ufw enable
  ```

---


## Experiment 2: Installing and Running Jupyter Notebook

### Overview

Jupyter Notebook is an interactive computing environment primarily used for Python programming. This experiment details how to install, configure, and make Jupyter accessible from another device.

### Steps

#### 1. Install Python and Pip

```bash
sudo apt update
sudo apt install python3 -y
sudo apt install python3-pip -y
```

#### 2. Verify Installation

```bash
python3 --version
pip3 --version
```

#### 3. Install Jupyter Notebook

```bash
pip3 install notebook
```

Launch Jupyter:

```bash
jupyter notebook
```

#### 4. Run Jupyter Notebook Remotely

To make Jupyter accessible from another device, run:

```bash
sudo apt install jupyter-core
jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser
```

**Example Output:**

![Jupyter Server Running](screenshots/jupyter_running.png)

#### 5. Find Local IP Address

Find your system's IP address to access Jupyter remotely:

```bash
hostname -I
```
or 

```bash
ip a
```

**Example Output:**

![Find Local IP](https://github.com/tarunkumar-sys/IOT406/blob/main/Screenshot%20(313).png)

#### 6. Allow Firewall Access (If Required)

If you have a firewall enabled, allow Jupyter's port:

```bash
sudo ufw allow 8888
```

#### 7. Access from Another Laptop

On a different device, enter the following in a web browser:

```
http://<your-ip>:8888
```

#### 8. Set Up a Password

To prevent unauthorized access, set a password:

```bash
jupyter notebook password
```

Follow the prompts to enter and confirm a password.

#### 9. Run Jupyter in Background

If you want Jupyter to keep running even after closing the terminal, use:

```bash
nohup jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser &
```

---


## Experiment 3: Setting Up Apache Tomcat and Deploying a Web Page

### Overview

Apache Tomcat is an open-source web server and servlet container designed for running Java-based web applications. This experiment walks you through installing Tomcat, setting up a basic web application, and managing files.

### Steps

#### 1. Install Java

Since Tomcat relies on Java, install OpenJDK 11 using the following commands:

```bash
sudo apt update
sudo apt install openjdk-11-jdk -y
```

Verify the installation:

```bash
java --version
```

#### 2. Download and Install Apache Tomcat

Fetch the latest version of Tomcat and extract it:

```bash
cd /opt
sudo wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.36/bin/apache-tomcat-10.1.36.tar.gz
sudo tar -xvzf apache-tomcat-10.1.36.tar.gz -C /opt
```

Rename the folder for simplicity:

```bash
sudo mv /opt/apache-tomcat-10.1.36 /opt/tomcat
```

Ensure script files are executable:

```bash
sudo chmod +x /opt/tomcat/bin/*.sh
```

#### 3. Start Apache Tomcat

Launch the Tomcat server:

```bash
sudo /opt/tomcat/bin/startup.sh
```

Verify by accessing:

```
http://localhost:8080
```

#### 4. Deploy a Web Page

Create a new directory inside the Tomcat `webapps` folder:

```bash
sudo mkdir /opt/tomcat/webapps/anime
```

Add web files:

```bash
sudo nano /opt/tomcat/webapps/anime/index.html
sudo nano /opt/tomcat/webapps/anime/style.css
sudo nano /opt/tomcat/webapps/anime/script.js
```

Restart Tomcat to apply changes:

```bash
sudo /opt/tomcat/bin/shutdown.sh
sudo /opt/tomcat/bin/startup.sh
```

Access the page:

```
http://localhost:8080/anime
```

#### 5. Managing Files

To remove a file:

```bash
sudo rm /opt/tomcat/webapps/anime/style.css
```

Confirm deletion:

```bash
ls /opt/tomcat/webapps/anime
```

---


## Experiment 4: Installing and Using Ollama

### Overview

Ollama allows you to run AI models locally for text generation, machine learning, and other AI-based applications. This section covers installing Ollama, running pre-trained models, and creating custom AI models.

### Steps

#### 1. Install Git and Curl

Before installing Ollama, ensure Git and Curl are installed:

```bash
sudo apt install git -y
sudo apt install curl -y
```

#### 2. Install Ollama

Download and install Ollama using the following command:

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

#### 3. Run a Pre-Trained Model

Ollama comes with pre-trained AI models. To run a model, use:

```bash
ollama run llama3:2.1b
```

**Example Output:**

![Ollama Running](https://github.com/tarunkumar-sys/IOT406/blob/main/Screenshot%20(312).png)

#### 4. Create a Custom AI Model

To create a personalized AI model, define a `Modelfile` with your desired parameters:

```bash
FROM llama3:2.1
PARAMETER temperature 1
SYSTEM """
You are Rias from High School DxD. Answer as Rias, the assistant, only.
"""
```

#### 5. Build and Use Custom Model

Once your `Modelfile` is ready, create and run your model:

```bash
ollama create rias -f ./Modelfile
ollama run rias
```

#### 6. List Installed Models

Check which models are installed on your system:

```bash
ollama ls
```

#### 7. Remove an Ollama Model

If you no longer need a model, remove it with:

```bash
ollama rm <model_name>
```

#### 8. Troubleshooting and Logs

If you encounter issues, check Ollama logs:

```bash
cat ~/.ollama/logs/latest.log
```

This helps diagnose errors and ensure smooth operation.

---


## Experiment 5: Setting Up Hadoop Environment

### Overview

Hadoop is an open-source framework that enables the distributed processing of large datasets across clusters of computers using simple programming models. This guide walks you through installing, configuring, and running Hadoop in a single-node environment.

### Steps

#### 1. Install Java (If Not Already Installed)

Hadoop requires Java. If you haven’t installed it yet, follow these steps:

```bash
sudo apt update
sudo apt install openjdk-11-jdk -y  # Or use openjdk-8-jdk if required
```

Verify Java installation:

```bash
java -version
```

If Java is not installed correctly, ensure that the correct version is set as the default:

```bash
sudo update-alternatives --config java
```

#### 2. Create Hadoop User

Create a dedicated user for running Hadoop to ensure proper file permissions and security:

```bash
sudo adduser hadoop
sudo usermod -aG sudo hadoop
```

Switch to the Hadoop user:

```bash
su - hadoop
```

Grant necessary permissions to Hadoop user:

```bash
sudo chown -R hadoop:hadoop /home/hadoop/
```
#### 3. Configure SSH (Required for Hadoop Operations)

```bash
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
ssh localhost
```

#### 4. Download Hadoop

Download the Hadoop tar file from the Apache Hadoop website or use wget:

```bash
wget https://downloads.apache.org/hadoop/common/hadoop-3.4.0/hadoop-3.4.0.tar.gz
```

Extract the Hadoop tar file:

```bash
sudo tar -xzvf hadoop-3.4.0.tar.gz
```

Rename the folder to 'hadoop':

```bash
sudo mv hadoop-3.4.0 hadoop
```

#### 5. Set Hadoop Environment Variables

Open the .bashrc file:

```bash
nano ~/.bashrc
```

Add the following environment variables:

```bash
export HADOOP_HOME=/home/hdoop/hadoop/
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
export HADOOP_OPTS="-Djava.libraray.path=$HADOOP_HOME/bin/native"
```

Apply the changes by running:

```bash
source ~/.bashrc
```

Check Current Directory for Hadoop:

```bash
cd $HADOOP_HOME
pwd
```

#### 6. Configure Hadoop

**chacking whrere is JAVA_HOME**

```bash
readlink -f $(which java)
```

For example, you might get an output like: 

```bash
/usr/lib/jvm/java-11-openjdk-amd64/bin/java
```

From this, 
you can extract the directory path without the /bin/java part, like:/usr/lib/jvm/java-11-openjdk-amd64

**Set Java Home in Hadoop Configuration**

```bash
cd $HADOOP_HOME/etc/hadoop
ls
cat hadoop-env.sh
sudo nano hadoop-env.sh
```

Update the following line:

```bash
export JAVA_HOME= /usr/lib/jvm/java-11-openjdk-amd64   
update-alternatives --config java 
```

Edit Hadoop configuration files (core-site.xml, hdfs-site.xml, mapred-site.xml, yarn-site.xml) as needed.

**Configure core-site.xml**

```bash
nano $HADOOP_HOME/etc/hadoop/core-site.xml
```

Add the following configuration:

```xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

**Configure hdfs-site.xml**

```bash
nano $HADOOP_HOME/etc/hadoop/hdfs-site.xml
```

Add the following configuration:

```xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```

**Configure mapred-site.xml**

```bash
nano $HADOOP_HOME/etc/hadoop/mapred-site.xml
```

Convert the template to a working file:

```bash
cp $HADOOP_HOME/etc/hadoop/mapred-site.xml.template $HADOOP_HOME/etc/hadoop/mapred-site.xml
```

Add the following configuration:

```xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```

**Configure yarn-site.xml**

```bash
nano $HADOOP_HOME/etc/hadoop/yarn-site.xml
```

Add the following configuration:

```xml
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
```

#### 7. Format the Hadoop Filesystem

Before starting Hadoop, format the NameNode:

```bash
hdfs namenode -format
```

#### 8. Start Hadoop Services

Start HDFS and YARN services:

```bash
start-dfs.sh
start-yarn.sh
```

To check the status of services:

```bash
hdfs dfsadmin -report
```

#### 9. Verify Hadoop Setup

Check the status of Hadoop services:

```bash
jps
```

You should see the following services running:

- NameNode
- DataNode
- ResourceManager
- NodeManager

Additionally, to access the Hadoop web UI:

- NameNode Web UI: [http://localhost:9870](http://localhost:9870)
- ResourceManager Web UI: [http://localhost:8088](http://localhost:8088)

To test the Hadoop installation, create a test directory in HDFS:

```bash
hdfs dfs -mkdir /test
hdfs dfs -ls /
```

---

## Hadoop COVID-19 Data Processing & Word File Analysis

### Download COVID-19 Dataset
Download the latest COVID-19 dataset from open data sources using `wget`:
```bash
wget https://raw.githubusercontent.com/owid/covid-19-data/master/public/data/latest/owid-covid-latest.csv -P /home/hadoop/
wget https://covid.ourworldindata.org/data/owid-covid-data.csv -P /home/hadoop/
wget https://github.com/CSSEGISandData/COVID-19/raw/master/csse_covid_19_data/csse_covid_19_daily_reports/01-01-2024.csv -P /home/hadoop/
```

### Upload Data to HDFS
```bash
hadoop fs -mkdir -p /user/hadoop/covid19
hadoop fs -put /home/hadoop/owid-covid-latest.csv /user/hadoop/covid19/
```
change the file name by the file you use "owid-covid-latest.csv"

### we will use here python code so wait
```bash
we will use here python code
```
we will use here python code

### Run MapReduce Job to Analyze Data
Example: Count cases by country using a MapReduce job:
```bash
hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.4.0.jar wordcount /user/hadoop/covid19/owid-covid-latest.csv /user/hadoop/covid19/output
```

### Verify Results
```bash
hadoop fs -cat /user/hadoop/covid19/output/part-r-00000
```

### Upload and Analyze a Word File to HDFS
#### Create a Directory in HDFS
```bash
hadoop fs -mkdir /user/hadoop/wordfiles
```
#### Upload the Word File to HDFS
```bash
hadoop fs -put /path/to/document.docx /user/hadoop/wordfiles/
```
#### Verify File in HDFS
```bash
hadoop fs -ls /user/hadoop/wordfiles/
```

### Convert the Word File to Text
Install Apache Tika for text extraction:
```bash
sudo apt-get install tika
```
Convert the Word file to text:
```bash
tika -t document.docx > document.txt
```

### Upload the Text File to HDFS
```bash
hadoop fs -put document.txt /user/hadoop/wordfiles/
```

### Run a Word Count MapReduce Job
```bash
hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.4.jar wordcount /user/hadoop/wordfiles/document.txt /user/hadoop/wordcount_output
```

### Check the Output
```bash
hadoop fs -cat /user/hadoop/wordcount_output/part-r-00000
```

---
## Step 3: Troubleshooting
### Check Logs
Logs are stored in `$HADOOP_HOME/logs/`. Example:
```bash
tail -f $HADOOP_HOME/logs/hadoop-hadoop-namenode-<hostname>.log
```

### Verify Hadoop Services
```bash
jps
```
If any of the required daemons (NameNode, DataNode, etc.) are not running, restart the services:
```bash
stop-dfs.sh
stop-yarn.sh
start-dfs.sh
start-yarn.sh
```

### Check Java Installation
```bash
echo $JAVA_HOME
```
Ensure it points to a valid Java installation directory.

### Verify Files in HDFS
```bash
hadoop fs -ls /user/hadoop/wordfiles/
```

### Run MapReduce Again
```bash
hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.4.jar wordcount /user/hadoop/wordfiles/document.txt /user/hadoop/wordcount_output
```

### Check Output Again
```bash
hadoop fs -cat /user/hadoop/wordcount_output/part-r-00000
```

---




