# Sonarqube Setup

SonarQube is an open-source static testing analysis software, it is used by developers to manage source code quality and consistency.
## 🧰 Prerequisites

Source: https://docs.sonarqube.org/latest/requirements/requirements/
1. An EC2 instance with a minimum of 2 GB RAM (t2.small)  
1. Java 11 installation   
   ```sh 
   amazon-linux-extras list
   amazon-linux-extras install java-openjdk11
   ```
1. SonarQube cannot be run as root on Unix-based systems, so create a dedicated user account for SonarQube if necessary.

## Installation steps

1. Download SonarQube [latest verions](https://www.sonarqube.org/downloads/) on to EC2 instace 
   ```sh 
   cd /opt  
   wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-x.x.zip  
   ```
1. extract packages
   ```sh 
   unzip /opt/sonarqube-x.x.zip
   ```

2. Change ownershipt to the user and Switch to Linux binaries directory to start service
   ```bash
   chown -R <sonar_user>:<sonar_user_group> /opt/sonarqube-x.x  
   cd /opt/sonarqube-x.x/bin/linux-x86-64   
   ./sonar.sh start
   ```
3. Connect to the SonarQube server through the browser. It uses port 9000.   
   `Note`: Port should be opened in the Security group 
   ```bash
   http://<Public-IP>:9000
   ```

   ## 🧹 CleanUp  
   1. Stop SonarQube server
   ```sh 
   cd /opt/sonarqube-x.x/bin/linux-x86-64 
   ./sonar.sh stop
   ```
   1. Terminate EC2 instance incase if you setup only for this lab. 

## 📌 Who is using this   
People who want to setup SonarQube and would like to integrate with Jenkins 
  ### 💡 Help/Suggestions or 🐛 Bugs

Thank you for your interest in contributing to our project. Whether it is a bug report, new feature, correction, or additional documentation or solutions, we greatly value feedback and contributions from our community. [Start here](/issues)   
