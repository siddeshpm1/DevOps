# Install Jenkins on AWS EC2
Jenkins is a self-contained Java-based program, ready to run out-of-the-box, with packages for Windows, Mac OS X and other Unix-like operating systems. As an extensible automation server, Jenkins can be used as a simple CI server or turned into the continuous delivery hub for any project.


### Follow this artical [On YouTube](https://youtu.be/ERR7cqW28FY)


### Prerequisites
1. EC2 Instance 
   - With Internet Access
   - Security Group with Port `8080` open for internet
1. Java 11 should be installed  


## Install Jenkins
 You can install jenkins using the rpm or by setting up the repo. We will set up the repo so that we can update it easily in the future.
1. Get the latest version of jenkins from https://pkg.jenkins.io/redhat-stable/ and install
   ```sh
   sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
	sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
	sudo yum upgrade
	# Add required dependencies for the jenkins package
	sudo yum install fontconfig java-17-openjdk
   	# sudo dnf install java-17-amazon-corretto -y
	sudo yum install jenkins
	sudo systemctl daemon-reload
   
   #on RedHat/CentOs 
   #yum install epel-release # repository that provides 'daemonize'
   #yum install java-11-openjdk-devel
   #yum install jenkins
   ```

# Start Jenkins

You can enable the Jenkins service to start at boot with the command:

```bash
sudo systemctl enable jenkins
```

You can start the Jenkins service with the command:

```bash
sudo systemctl start jenkins
```

You can check the status of the Jenkins service using the command:

```bash
sudo systemctl status jenkins
```

If everything has been set up correctly, you should see an output like this:

```
Loaded: loaded (/lib/systemd/system/jenkins.service; enabled; vendor preset: enabled)
Active: active (running) since Tue 2023-06-22 16:19:01 +03; 4min 57s ago

```
   ### Accessing Jenkins
   By default jenkins runs at port `8080`, You can access jenkins at
   ```sh
   http://YOUR-SERVER-PUBLIC-IP:8080
   ```
  #### Configure Jenkins
- The default Username is `admin`
- Grab the default password 
- Password Location:`/var/lib/jenkins/secrets/initialAdminPassword`
- `Skip` Plugin Installation; _We can do it later_
- Change admin password
   - `Admin` > `Configure` > `Password`
- Configure `java` path
  - `Manage Jenkins` > `Global Tool Configuration` > `JDK`  
- Create another admin user id

### Test Jenkins Jobs
1. Create “new item”
1. Enter an item name – `My-First-Project`
   - Chose `Freestyle` project
1. Under the Build section
	Execute shell: echo "Welcome to Jenkins Demo"
1. Save your job 
1. Build job
1. Check "console output"

