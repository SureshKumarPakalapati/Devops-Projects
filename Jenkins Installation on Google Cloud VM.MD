# Jenkins Installation on Google Cloud VM
Jenkins is a self-contained Java-based program, ready to run out-of-the-box, with packages for Windows, Mac OS X and other Unix-like operating systems. As an extensible automation server, Jenkins can be used as a simple CI server or turned into the continuous delivery hub for any project.

### Prerequisites
1. GC CentOS 7 Instance 
   - With Internet Access
   - Security Group with Port `8080` open for internet
1. Java v1.8.x 

## Install Java
We will be using open java for our demo, Get latest version from http://openjdk.java.net/install/
```sh
yum install java-1.8*
#yum -y install java-1.8.0-openjdk
```

### Confirm Java Version
Lets install java and set the java home
```sh
java -version
find /usr/lib/jvm/java-1.8* | head -n 3
#JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.191.b12-1.el7_6.x86_64
export JAVA_HOME
PATH=$PATH:$JAVA_HOME
# To set it permanently update your .bash_profile
source ~/.bash_profile
```
_The output should be something like this,_
```
[root@~]# java -version
openjdk version "1.8.0_151"
OpenJDK Runtime Environment (build 1.8.0_151-b12)
OpenJDK 64-Bit Server VM (build 25.151-b12, mixed mode)
```

## Install Jenkins
You can install jenkins using the rpm or by setting up the repo. We will setup the repo so that we can update it easily in future.
Get latest version of jenkins from https://pkg.jenkins.io/redhat-stable/
```sh
yum -y install wget
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
yum -y install jenkins
```

### Start Jenkins
```sh
# Start jenkins service
systemctl start jenkins

# Setup Jenkins to start at boot,
systemctl enable jenkins
```

#### Accessing Jenkins
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
1. Under Build section
	Execute shell : echo "Welcome to Jenkins Demo"
1. Save your job 
1. Build job
1. Check "console output"


# Ansible integration with Jenkins

### Prerequisites:
1. Ansible server
2. Jenkins Server

### Integration Setps

Install "publish Over SSH"
 - `Manage Jenkins` > `Manage Plugins` > `Available` > `Publish over SSH` 

Enable connection between Ansible and Jenkins
- `Manage Jenkins` > `Configure System` > `Publish Over SSH` > `SSH Servers` 

	- SSH Servers:
		- Hostname:`<ServerIP>`
		- username: `ansadm`
		- password: `*******`

Test the connection "Test Connection"


# Configure Git pulgin on Jenkins
Git is one of the most popular tool for version contorl system. you can pull code from git repositories using jenkins if you use github plugin. 

#### Prerequisites
1. Jenkins server

#### Install Git on Jenkins server
Install git packages on jenkins server
```sh
  yum install git -y
 ```

#### Setup Git on jenkins console
- Install git plugin without restart  
  - `Manage Jenkins` > `Jenkins Plugins` > `available` > `github`

- Configure git path
  - `Manage Jenkins` > `Global Tool Configuration` > `git`

#  Install & configure Maven build tool on Jenkins
Maven is a code build tool which used to convert your code to artifact. this is widely used plugin to build in continuous integration

#### Prerequisites
1. Jenkins server

#### Install Maven on Jenkins
Download maven packages https://maven.apache.org/download.cgi onto Jenkins server. In this case I am using /opt/maven as my installation directory
	- Link : https://maven.apache.org/download.cgi
```sh
  # Creating maven directory under /opt
  mkdir /opt/maven
  cd /opt/maven
  # downloading maven version 3.6.0
  wget http://mirrors.fibergrid.in/apache/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.zip
  unzip /opt/maven/apache-maven-3.6.0-bin.zip
 ```
	
Setup M2_HOME and M2 paths in .bash_profile of user and add these to path variable
```sh
  vi ~/.bash_profile
  M2_HOME=/opt/maven/apache-maven-3.6.0
  M2=$M2_HOME/bin
  PAHT=<Existing_PATH>:$M2_HOME:$M2
```
#### Check point 
logoff and login to check maven version
Check maven version 
```sh
  mvn –version
```
So far you have completed installation of maven software to support maven plugin on jenkins console. Let's jump onto jenkins to complete remining steps. 

#### Setup maven on jenkins console
- Install maven plugin without restart  
  - `Manage Jenkins` > `Jenkins Plugins` > `available` > `Maven Invoker`

- Configure java path
  - `Manage Jenkins` > `Global Tool Configuration` > `Maven`
  
### Next Step
- [x] Configure Users & Groups in Jenkins
- [x] Secure your Jenkins Server
- [x] Jenkins Master-Slave Configuration
- [x] Setup Jenkins to run inside Tomcat Server

