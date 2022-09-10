Maven Installation Script
#!/bin/sh
# Create a redhat server in aws with at least 4GB of RAM
# sudo -i = become the root users
sudo hostname maven
cd /opt
# install Java JDK 1.8+ as a pre-requisit for maven to run.

sudo hostname maven
cd /opt
sudo yum install wget nano tree unzip git-all -y
sudo yum install java-11-openjdk-devel java-1.8.0-openjdk-devel -y
java -version
git --version

#Step1) Download the Maven Software
sudo wget https://dlcdn.apache.org/maven/maven-3/3.8.5/binaries/apache-maven-3.8.5-bin.zip
sudo unzip apache-maven-3.8.5-bin.zip
sudo rm -rf apache-maven-3.8.5-bin.zip
sudo mv apache-maven-3.8.5/ maven

#Step3) Set Environmental Variable  -  For Specific User
----------------------
#vi ~/.bash_profile
echo "export M2_HOME=/opt/maven"   >>  ~/.bash_profile
echo "export PATH=$PATH:$M2_HOME/bin"   >> ~/.bash_profile

#refresh the .bash_profile file
source ~/.bash_profile
mvn -version

Apache Tomcat Installation
#!/bin/bash
# Use this script to install tomcat in rehat servers
echo delete the failed version of tomcat
sudo rm -rf /opt/tomcat9
echo assign a hostname to your server 
sudo hostname tomcat2
# install Java JDK 1.8+ as a pre-requisit for tomcat to run.
cd /opt 
sudo yum install git wget -y
sudo yum install java-1.8.0-openjdk-devel -y
# Download tomcat software and extract it.
sudo yum install wget unzip -y

sudo wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.62/bin/apache-tomcat-9.0.62.tar.gz
sudo tar -xvf apache-tomcat-9.0.62.tar.gz
sudo rm apache-tomcat-9.0.62.tar.gz
sudo mv apache-tomcat-9.0.62 tomcat9
sudo chmod 777 -R /opt/tomcat9
sudo chown ec2-user -R /opt/tomcat9
sh /opt/tomcat9/bin/startup.sh
# create a soft link to start and stop tomcat
sudo ln -s /opt/tomcat9/bin/startup.sh /usr/bin/starttomcat
sudo ln -s /opt/tomcat9/bin/shutdown.sh /usr/bin/stoptomcat
sudo yum update -y
starttomcat



NEXUS INSTALLATION
#!/bin/bash
# Install and start nexus as a service 
# This script works on RHEL 7 & 8 OS 
# Your server must have atleast 4GB of RAM
# become the root / admin user via: sudo su -
#1. Create nexus user to manage the nexus
# As a good security practice, Nexus is not advised to run nexus service as a root user, so create a new user called nexus and grant sudo access to manage nexus services as follows.
useradd nexus
#4 Give sudo access to nexus user
sudo echo "nexus ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/nexus
sudo su - sonar
cd /opt
# 1.Install prerequisit: JAVA, git, unzip
sudo yum install wget git nano unzip -y
sudo yum install java-11-openjdk-devel java-1.8.0-openjdk-devel -y
# 2. Download nexus software and extract it (unzip)
sudo wget http://download.sonatype.com/nexus/3/nexus-3.15.2-01-unix.tar.gz 
sudo tar -zxvf nexus-3.15.2-01-unix.tar.gz
mv /opt/nexus-3.15.2-01 /opt/nexus
#5 Change the owner and group permissions to /opt/nexus and /opt/sonatype-work directories.
sudo chown -R nexus:nexus /opt/nexus
sudo chown -R nexus:nexus /opt/sonatype-work
sudo chmod -R 775 /opt/nexus
sudo chmod -R 775 /opt/sonatype-work
#6 Open /opt/nexus/bin/nexus.rc file and  uncomment run_as_user parameter and set as nexus user.
vi /opt/nexus/bin/nexus.rc
run_as_user="nexus"
#7 CONFIGURE NEXUS TO RUN AS A SERVICE 
sudo ln -s /opt/nexus/bin/nexus /etc/init.d/nexus
#9 Enable and start the nexus services
sudo systemctl enable nexus
sudo systemctl start nexus
sudo systemctl status nexus
echo "end of nexus installation"

=====================
access nexus on the browser
34.229.62.93:8081
userName  --- ADMIN
Password   ADMIN123

<<Troubleshooting
---------------------
nexus service is not starting?

a)make sure  to change the ownership and group to /opt/nexus and /opt/sonatype-work directories and permissions (775) for nexus user.
b)make sure you are trying to start nexus service AS nexus user.
c)check java is installed or not using java -version command.
d) check the nexus.log file which is availabe in  /opt/sonatype-work/nexus3/log  directory.

Unable to access nexus URL?
-------------------------------------
a)make sure port 8081 is opened in security groups in AWS ec2 instance.
                  
 
                  
JENKINS INSTALLATION SCRIPT
  Jenkins-install.sh  
# CREATE HOSTNAME 
sudo hostname auto  

sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
sudo yum upgrade
# Add required dependencies for the jenkins package
sudo yum install java-11-openjdk -y 
sudo yum install jenkins -y 
sudo systemctl daemon-reload
# start jenkins 
# Start Jenkins
# You can enable the Jenkins service to start at boot with the command:
sudo systemctl enable jenkins
#You can start the Jenkins service with the command:=
sudo systemctl start jenkins
# You can check the status of the Jenkins service using the command:
sudo systemctl status jenkins                

    
    DOCKER-JENKINS INSTALLATION IN UBUNTU SERVER              
   /bin/bash
# Run this as a script
sudo hostname docker
sudo apt install docker.io -y
sudo usermod -aG docker ubuntu 
# Install java as jenkins dependency
sudo apt install openjdk-11-jdk -y
# install jenkis in ubuntu:
sudo wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins -y 
sudo systemctl start jenkins 

echo "jenkins  ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/jenkins" 
sudo echo "jenkins:admin" | chpasswd
sudo sed -i "/^[^#]*PasswordAuthentication[[:space:]]no/c\PasswordAuthentication yes" /etc/ssh/sshd_config
sudo service sshd restart
sudo usermod -aG  docker jenkins             
                  
==============================================================================================================================
Landmark Technologies Canada/USA, Ph: + 1 437 215 2483
==============================================================================================================================

Agenda: Kubernetes Setup Using Kubeadm In AWS EC2 Ubuntu Servers
=======


Prerequisite:
=============

3 - Ubuntu Servers -- 18.04
1 - Manager (4GB RAM , 2 Core)  t2.medium
2 - Workers  (1 GB, 1 Core)     t2.micro


Note: Open Required Ports In AWS Security Groups. 
      For now we will open All trafic.

==========COMMON FOR MASTER & WORKER NODES START ====

# First, login as ‘root’ user because the following set of commands need to be executed with ‘sudo’ permissions.

sudo hostname k8s
sudo su -  OR sudo -i

=# Install Required packages and apt keys.

sudo apt-get update -y
sudo apt-get install -y apt-transport-https

sudo curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

apt-get update -y

#Turn Off Swap Space

swapoff -a
sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab


#Install kubeadm, Kubelet And Kubectl containerd

sudo apt-get install -y kubelet containerd kubeadm kubectl kubernetes-cni 

# apt-mark hold will prvent the package from being authomatically upgraded or removed

sudo apt-mark hold kubelet containerd kubeadm kubectl kubernetes-cni 


# helm install javaapp

cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF

# Setup required sysctl params, these persist across reboots.
cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF

# Apply sysctl params without reboot
sudo sysctl --system

# Configure containerd:

sudo mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml

# Restart containerd:
sudo systemctl restart containerd

# If you get error releated to kubernetes-cni if alreay exists install with out kubernetes-cni
apt-get install -y kubelet kubeadm kubectl 

# Enable and start kubelet service

sudo systemctl daemon-reload 
sudo systemctl start kubelet 
sudo systemctl enable kubelet.service

sudo kubeadm join 172.31.30.188:6443 --token pzxjka.sylger2rnt0i5bpo \
        --discovery-token-ca-cert-hash sha256:51f95904b1324fa1328164660419cfb8a644d5bd0ecf18ec283370970d171b27

==========COMMON FOR MASTER & SLAVES END=====

SELF-MANAGED K8s Cluster Installation(kubeadm)

===========In Master Node Start====================
# Steps Only For Kubernetes Master

# Switch to the root user.

sudo su -

# Initialize Kubernates master by executing below commond.

sudo kubeadm init

#exit as root user & exeucte as normal ubuntu user

sudo su - ubuntu

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config


# To verify, if kubectl is working or not, run the following command.

kubectl get pods -o wide -n kube-system

#You will notice from the previous command, that all the pods are running except one: ‘core-dns’. For resolving this we will install a # pod network. To install the weave pod network, run the following command:

kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"

kubectl get nodes

kubectl get pods 
kubectl get pods --all-namespaces


# Get token



=========In Master Node End====================


Add Worker Machines to Kubernates Master
=========================================

Copy kubeadm join token from and execute in Worker Nodes to join to cluster

kubeadm join 172.31.10.12:6443 --token cdm6fo.dhbrxyleqe5suy6e \
        --discovery-token-ca-cert-hash sha256:1fc51686afd16c46102c018acb71ef9537c1226e331840e7d401630b96298e7d


kubectl commonds has to be executed in master machine.

Check Nodes 
=============

kubectl get nodes


Deploy Sample Application
==========================

kubectl run nginx-demo --image=nginx --port=80 

kubectl expose deployment nginx-demo --port=80 --type=NodePort
