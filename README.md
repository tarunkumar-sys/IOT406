# Experiments with Apache Tomcat, Jupyter Notebook, and Ollama

Welcome to this repository, where we explore various experiments involving Apache Tomcat, Jupyter Notebook, and Ollama. Whether you're a beginner or an experienced user, these step-by-step instructions will guide you through setting up, deploying, and managing different technologies effectively.

## Experiment 1: Setting Up Apache Tomcat and Deploying a Web Page

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

## Experiment 3: Installing and Using Ollama

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

## Experiment 4: Setting Up Hadoop Environment

### Overview

Hadoop is an open-source framework that enables the distributed processing of large datasets. This experiment guides you through installing, configuring, and running Hadoop in a single-node setup.

### Steps

#### 1. Install Java (If Not Already Installed)

Hadoop requires Java. If you haven’t installed it yet, follow these steps:

```bash
sudo apt update
sudo apt install openjdk-11-jdk -y
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

#### 3. Download Hadoop

Download the Hadoop tar file from the Apache Hadoop website or use wget:

```bash
wget https://downloads.apache.org/hadoop/common/hadoop-3.4.0/hadoop-3.4.0.tar.gz
```

Extract the Hadoop tar file:

```bash
sudo tar -xzvf hadoop-3.4.0.tar.gz -C /home/hadoop/
```

Rename the folder to 'hadoop':

```bash
sudo mv /home/hadoop/hadoop-3.4.0 /home/hadoop/hadoop
```

#### 4. Set Hadoop Environment Variables

Open the .bashrc file:

```bash
nano ~/.bashrc
```

Add the following environment variables:

```bash
export HADOOP_HOME=/home/hadoop/hadoop
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$JAVA_HOME/bin
```

Apply the changes by running:

```bash
source ~/.bashrc
```

#### 5. Configure Hadoop

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

#### 6. Format the Hadoop Filesystem

Before starting Hadoop, format the NameNode:

```bash
hdfs namenode -format
```

#### 7. Start Hadoop Services

Start HDFS and YARN services:

```bash
start-dfs.sh
start-yarn.sh
```

To check the status of services:

```bash
hdfs dfsadmin -report
```

#### 8. Verify Hadoop Setup

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



