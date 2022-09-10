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
