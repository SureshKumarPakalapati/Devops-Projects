# Devops-Projects
#### Jenkins, Ansible, Tomcat Installation from scratch

#### Simple DevOps Project-1 | Simple DevOps project for CI/CD | CI/CD through Jenkins
We know how to use work with each and Git, Jenkins independently. What if you want to collaborate these two? that is where Simple DevOps project helps you. Follow below steps if you are a new guy to DevOps. You love it. 

#### Prerequisites
1. GC instance with tomcat installation
1. Jenkins server

### Adding steps for Integration
### Steps to create Jenkin job
1. Login to Jenkins console
1. Create *Jenkins job*, Fill the following details,
   - *Source Code Management:*
      - Repository: `https://github.com/ValaxyTech/hello-world.git`
      - Branches to build : `*/master`  
   - *Build:*
     - Root POM:`pom.xml`
     - Goals and options : `clean install package`

### Adding Deployment Steps 
in this port we are going to install 'deploy to container' plugin. this is need to deploy on tomcat server which we are using. 

- Install maven plugin without restart  
  - `Manage Jenkins` > `Jenkins Plugins` > `available` > `deploy to container`
 
To deploy our build artifacts on tomcat server our Jenkins server need access. For this we should setup credentials. This option is available in Jenkins home page

- setup credentials
  - `credentials` > `jenkins` > `Global credentials` > `add credentials`
    - Username	: `deployer`
    - Password : `XXXXXXX`
    - id      :  `Tomcat_user`
    - Description: `Tomcat user to deploy on tomcat server`

Modify the same job which created in part-01 and add deployment steps.
 - Post Steps
   - Deploy war/ear to container
      - WAR/EAR files : `**/*.war`
      - Containers : `Tomcat 8.x`
         - Credentials: `Tomcat_user` (which created in above step)
         - Tomcat URL : `http://<PUBLIC_IP>:<PORT_NO>`

Save and run the job now.

### Continuous Integration & Continuous Deployment (CI/CD)
Now job is running fine but to make this as Continuous Integration and Continuous Deployment Tod do that go back and modify job as below. 
  - Build Triggers
    - Poll SCM
      - schedule `*/2 * * * *`

Save the job and modify the code in GitHub. Then you could see your job get trigger a build without any manual intervention.


#### Simple DevOps Project-2 | CI/CD pipeline using GIT, Jenkins & Ansible

