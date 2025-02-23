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

**Example Output:**
![Find Local IP](screenshots/find_ip.png)

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
![Ollama Running](screenshots/ollama_running.png)

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

**Example Log Output:**
![Ollama Logs](screenshots/ollama_logs.png)

This helps diagnose errors and ensure smooth operation.

