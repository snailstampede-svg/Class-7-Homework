# Jenkins Server Setup
---
## Setup Steps
1. Create Security Group
2. Prepare launch script for a basic Jenkins Server
3. Create EC2
4. Test the EC2 instance
5. Log in to Jenkins Server
6. Confirm configuration

## Step 1 - Create Security Group
1. In AWS Console, search for EC2 and scroll down to Security Groups in the left pane.
2. Under Network & Security , click on Security Groups.
3. Click on Create esecurity group and give your security group a name and description
4. Under Inbound rules, click on Add rule and create two (2)  inbound rules as follows:

### Rule #1
    - Type: SSH
    - Protocol: TCP
    - Port Range: 22
    - Source: Anywhere- IPv4
    - CIDR Blocks: 0.0.0.0/0
    - Description: SSH


### Rule #2
    - Type: Custom TCP
    - Protocol: TCP
    - Port Range: 8080
    - Source: Anywhere- IPv4
    - CIDR Blocks: 0.0.0.0/0
    - Description: Jenkins

5. Click on Create security group 

## Step 2 - Prepare launch script for a basic Jenkins Server
Below is the script to install and configure all the for the EC2 instance. This is what the Ec2 will load on first run.

There are a few changes that were made to a base script in order to meet project requirements. 
- Upgrade from Java 17 to Java 21

- Script out adding the jenkins plugins, so that when the server is built and ready for interaction, the necessary plugins are already installed 

- Ensure that there is enough space in /tmp to prevent Jenkins Disk Space errors

```#!/bin/bash
######### Start- Handle Disk Space Issues #########
# Check current space for debugging in logs
df -h /tmp
mount | grep /tmp

# Increase /tmp size to 4G to prevent Jenkins 'Disk Space' errors
# This is applied before Jenkins starts to ensure the workspace is ready

sudo mount -o remount,size=4G /tmp

######### End- Handle Disk Space Issues #########

# --------------------------------------
# Update all installed packages
# --------------------------------------
sudo yum update -y

# --------------------------------------
# Add the Jenkins repository to yum sources
# --------------------------------------
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

# --------------------------------------
# Import the Jenkins GPG key to verify packages
# --------------------------------------
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

# --------------------------------------
# Upgrade all packages (including those from the new Jenkins repo)
# --------------------------------------
sudo yum upgrade -y

# --------------------------------------
# Install Amazon Corretto 17 (required Java version for Jenkins)
# --------------------------------------
# -sudo yum install java-17-amazon-corretto -y # We will be migratin to Java 21 below

#### # Install Java 21 (Amazon Corretto)
sudo yum install java-21-amazon-corretto -y

# --------------------------------------
# Install Jenkins
# --------------------------------------
sudo yum install jenkins -y

# --------------------------------------
# Enable Jenkins to start at boot
# --------------------------------------
sudo systemctl enable jenkins

# --------------------------------------
# Start the Jenkins service
# --------------------------------------
sudo systemctl start jenkins

#---------------------------------------
# Install Git
#---------------------------------------
sudo yum install git -y

# Download Jenkins Plugin Manager
wget https://github.com/jenkinsci/plugin-installation-manager-tool/releases/latest/download/jenkins-plugin-manager.jar

# Install the extensive list of AWS, Google, and Security plugins
sudo java -jar jenkins-plugin-manager.jar --war /usr/share/java/jenkins.war \
--plugin-download-directory /var/lib/jenkins/plugins/ \
--plugins \
aws-credentials pipeline-aws ec2 amazon-ecs aws-codedeploy aws-lambda \
aws-codebuild s3 aws-secrets-manager-credentials-provider aws-codepipeline \
configuration-as-code-aws-ssm cloudformation aws-sam terraform kubernetes \
google-cloud-storage google-kubernetes-engine google-oauth-plugin \
google-cloud-sdk snyk-security-scanner sonar aqua-security-scanner \
aqua-microscanner aqua-serverless github github-oauth pipeline-github-lib \
pipeline-githubnotify-step maven-plugin pipeline-maven publish-over-ssh

# --- 5. Final Permission Fix & Restart ---
sudo chown -R jenkins:jenkins /var/lib/jenkins/plugins/
sudo systemctl restart jenkins

# Verify space one last time in the EC2 System Log
df -h /tmp


# --------------------------------------
# Optional: Check the status of Jenkins (won’t display in EC2 user data logs but useful for debugging)
# --------------------------------------
sudo systemctl status jenkins

```

## Step 3 - Prepare launch script

1. In the EC2 Console, under instances click on Instances and then click on Launch Instance.
2. Provide a name for the instance, select Amazon Linux for the OS and select t3 small for the instance type.
3. Select a previously created key pair or create a new one. This is critical to access the instance via SSH for any configuration or management issues.
4. Under Network Settings, click on Edit and select the following:
    - VPC: Feel free to use the Default VPC or one specifically created for the Jenkins server.
    - Subnet and availability zone
    - Ensure Auto-assign public IP is Enabled.
    - Select Existing Security Group - Select the previously created security group with specific Jenkins rules.
5. Under Configure storage, change the root volume to 20 GiB
6.  Click on Advanced details and scroll down to the User data text box. Paste the above script into the text box.
7. Click on Launch instance

## Step 4 - Test the EC2 instance

1. In the Ec2 window, select your newly created instance and  wait a few moments for the status check in the console to show "3/3 checks passed"
2. Once the checks have passed, click on the double-square icon to copy the Publick IPv4 addres.
3. Open a new browser window and in the address bar type: 
```
http://<public-IPv4 address>:8080
```
4. Hit enter and the Jenkins web pages should open to a page showing Unlock Jenkins with a text box to enter the Administrator password. 


## Step 5 - Log in to Jenkins Server

1. To retrieve the Administrator password for Jenkin, SSH into the Ec2 instance.
    - to ssh into the instance, go to the instance in the console. 
    - select the instance and click on connect. 
    - on the subsequent page, click on tab marked SSH client and follow the instructions. 

2. Once logged in to the EC2 instance command line, type the following command to retrieve the default administratore password for your Jenkins server.
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

3. The command will produce a text string. Copy this string, paste it into the text box on your Jenkins server login page and click on Continue.

4. You are presented with the Getting Started page. Click on Install suggested plugins and wait for the plugins to install.

5. Once the plugins are done installing, you will be asked to create the First Admin User. Enter the credentials for this user and click on Save and Continue.
6. You are presented with an Instance Configuration page which shows the URL of the Jenkins server. Copy and bookmark this url. Click on Save and Finish.
7. You are presented with the final page stating that "Jenkins is ready". Click on Start using Jenkins.


## Step 6 - Confirm configuration

