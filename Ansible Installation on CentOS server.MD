# Ansible Installation on CentOS server

# Install Python latest version (on Control node and Managed host)

yum install python3 -y

By default, python3 is the command to run python commands. to use just python, use "alternatives" command. (on Cont
rol node and Managed host)

alternatives --set python /usr/bin/python3

Check for Python version

python --version

# Install python-pip package manager (on Control node)

yum -y install python3-pip

# Create a new user for ansible administration & grant admin access to the user (on Control node and Managed host)

useradd ansadmin

passwd ansadmin

# Below command adds ansadmin to sudoers file. But we strongly recommended using "visudo" command if you are aware vi or nano editor. (on Control node and Managed host)

echo "ansadmin ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

Using key-based authentication is advised. If you are still at the learning stage use password-based authentication
 (on Control node and Managed host)
 
# sed command replaces "PasswordAuthentication no to yes" without editing file 

 sed -ie 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
 
 sudo service sshd reload
 
# Install Ansible as a ansadmin user (on Control node)

su - ansadmin
pip3 install ansible --user

Note: Ansible must be installed as a user (here ansadmin)

check for ansible version

ansible --version

sudo mkdir /etc/ansible

sudo vi /etc/ansible/hosts
localhost

# Log in as a ansadmin user on master and generate ssh key (on Control node)
ssh-keygen

Copy keys onto all ansible managed hosts (on Control node)

ssh-copy-id ansadmin@<target-server>
  
Validation test

Create a directory /etc/ansible and create an inventory file called "hosts" add control node IP address in it.

# Run ansible command as ansadmin user it should be successful (Master)

ansible all -m ping
