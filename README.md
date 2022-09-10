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
