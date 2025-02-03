# Tutorial

## How To Install Apache Tomcat 10 on Ubuntu 20.04

**Java | Ubuntu 20.04 | Apache**  
**Authors:** Siddesh P M

---

## Introduction

Apache Tomcat is a web server and servlet container that is used to serve Java applications. It’s an open-source implementation of the Jakarta Servlet, Jakarta Server Pages, and other technologies of the Jakarta EE platform.

In this tutorial, you’ll deploy Apache Tomcat 10 on Ubuntu 20.04. You will install Tomcat 10, set up users and roles, and navigate the admin user interface.

---

## Prerequisites

- One Ubuntu 20.04 server with a sudo non-root user and a firewall, which you can set up by following the Ubuntu 20.04 Initial Server Setup.

---

## Step 1 — Installing Tomcat

In this section, you will set up Tomcat 10 on your server. To begin, you will download its latest version and set up a separate user and appropriate permissions for it. You will also install the Java Development Kit (JDK).

For security purposes, Tomcat should run under a separate, unprivileged user. Run the following command to create a user called `tomcat`:

```bash
sudo useradd -m -d /opt/tomcat -U -s /bin/false tomcat
```

By supplying `/bin/false` as the user’s default shell, you ensure that it’s not possible to log in as `tomcat`.

### Install the JDK

First, update the package manager cache:

```bash
sudo apt update
```

Then, install the JDK:

```bash
sudo apt install default-jdk
```

Answer `y` when prompted to continue with the installation.

Verify the Java installation:

```bash
java -version
```

**Expected Output:**

```bash
openjdk version "11.0.14" 2022-01-18
OpenJDK Runtime Environment (build 11.0.14+9-Ubuntu-0ubuntu2.20.04)
OpenJDK 64-Bit Server VM (build 11.0.14+9-Ubuntu-0ubuntu2.20.04, mixed mode, sharing)
```

### Download and Install Tomcat

Navigate to the `/tmp` directory:

```bash
cd /tmp
```

Download the latest Tomcat 10 Core Linux build:

```bash
wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.34/bin/apache-tomcat-10.1.34.tar.gz
```

Extract the archive:

```bash
sudo tar xzvf apache-tomcat-10*tar.gz -C /opt/tomcat --strip-components=1
```

Set ownership and permissions:

```bash
sudo chown -R tomcat:tomcat /opt/tomcat/
sudo chmod -R u+x /opt/tomcat/bin
```

---

## Step 2 — Configuring Admin Users

Edit the Tomcat users configuration file:

```bash
sudo nano /opt/tomcat/conf/tomcat-users.xml
```

Add the following lines before the closing `</tomcat-users>` tag:

```xml
<role rolename="manager-gui" />
<user username="manager" password="manager_password" roles="manager-gui" />

<role rolename="admin-gui" />
<user username="admin" password="admin_password" roles="manager-gui,admin-gui" />
```

Replace `manager_password` and `admin_password` with your own values. Save and close the file.

To remove access restrictions, edit the `context.xml` file for the Manager app:

```bash
sudo nano /opt/tomcat/webapps/manager/META-INF/context.xml
```

Comment out the following line:

```xml
<!--  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\..*" /> -->
```

Repeat this step for the Host Manager:

```bash
sudo nano /opt/tomcat/webapps/host-manager/META-INF/context.xml
```

---

## Step 3 — Creating a systemd Service

Find the Java installation path:

```bash
sudo update-java-alternatives -l
```

Expected Output:

```bash
java-1.11.0-openjdk-amd64      1111       /usr/lib/jvm/java-1.11.0-openjdk-amd64
```

Create a systemd service file:

```bash
sudo nano /etc/systemd/system/tomcat.service
```

Add the following content:

```ini
[Unit]
Description=Tomcat
After=network.target

[Service]
Type=forking

User=tomcat
Group=tomcat

Environment="JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64"
Environment="CATALINA_HOME=/opt/tomcat"
Environment="CATALINA_BASE=/opt/tomcat"
Environment="CATALINA_PID=/opt/tomcat/temp/tomcat.pid"
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
```

Save and close the file.

Reload the systemd daemon:

```bash
sudo systemctl daemon-reload
```

Start Tomcat:

```bash
sudo systemctl start tomcat
```

Check the status:

```bash
sudo systemctl status tomcat
```

Enable Tomcat to start on boot:

```bash
sudo systemctl enable tomcat
```

---

## Step 4 — Accessing the Web Interface

Allow traffic to Tomcat’s port:

```bash
sudo ufw allow 8080
```

Access Tomcat in your browser:

```
http://your_server_ip:8080
```

Log in using the credentials you set in Step 2.

### Manager App:

Here, you can start, stop, reload, deploy, and undeploy Java applications.

### Host Manager:

Use this interface to manage virtual hosts.

---

## Conclusion

In this tutorial, you installed Apache Tomcat 10 on Ubuntu 20.04, created an admin user, configured a systemd service, and accessed the Tomcat web interface.

You’re now ready to deploy Java applications on your Tomcat server!

# How To Set Up SSH Key-Based Authentication on an Ubuntu Server

## Step 1 — Creating the Key Pair

The first step is to create a key pair on the client machine (usually your computer):

```shell
ssh-keygen
```
By default, recent versions of ssh-keygen will create a 3072-bit RSA key pair. For enhanced security, you can create a 4096-bit key using:

```
ssh-keygen -b 4096
```
After entering the command, you'll see:

```
Generating public/private rsa key pair.
Enter file in which to save the key (/your_home/.ssh/id_rsa):
```
Press Enter to use the default location or specify an alternate path.

Security Note: If an existing key is detected:

```
/home/your_home/.ssh/id_rsa already exists.
Overwrite (y/n)?
```
Overwriting will make previous keys unusable. Proceed with caution.

You'll then be prompted for a passphrase (recommended for added security):

```
Enter passphrase (empty for no passphrase):
```
After completion, you'll see:

```
Your identification has been saved in /your_home/.ssh/id_rsa
Your public key has been saved in /your_home/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:/hk7MJ5n5aiqdfTVUZr+2Qt+qCiS7BIm5Iv0dxrc3ks user@host
The key's randomart image is:
+---[RSA 3072]----+
|                .|
|               + |
|              +  |
| .           o . |
|o       S   . o  |
| + o. .oo. ..  .o|
|o = oooooEo+ ...o|
|.. o *o+=.*+o....|
|    =+=ooB=o.... |
+----[SHA256]-----+
```
## Step 2 — Copying the Public Key to Your Ubuntu Server
** Copying the Public Key Manually **
Display your public key:

```
cat ~/.ssh/id_rsa.pub
```
Example output:

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCqql6MzstZYh1TmWWv... demo@test
```
On your server, create SSH directory (if needed):

```
mkdir -p ~/.ssh
```
Add public key to authorized keys:

```
echo public_key_string >> ~/.ssh/authorized_keys
```
Set proper permissions:

```
chmod -R go= ~/.ssh
```
Ensure correct ownership (replace sammy with your username):
```
chown -R sammy:sammy ~/.ssh
```
**Step 3 — Authenticating to Your Ubuntu Server Using SSH Keys**
Connect using:

```
ssh username@remote_host
```
First connection warning (type yes to continue):

```
The authenticity of host '203.0.113.1 (203.0.113.1)' can't be established...
Are you sure you want to continue connecting (yes/no)? yes
```
If using a passphrase, you'll be prompted to enter it.

Successful authentication opens a new shell session.

Security Recommendation: After verifying key-based authentication works, consider disabling password authentication for enhanced security.

