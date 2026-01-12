Installation of Maven on Linux Server
Note: For the latest releases and more information, visit the official website: https://maven.apache.org/download.cgi

To install Maven on a Linux server, follow these steps:

Prerequisites: Ensure that your user has sudo privileges to install packages.

Maven Version: Determine the version of Maven you wish to install. For this example, we will use version 3.9.9.

Install Java 17

````
sudo apt update -y
sudo apt install openjdk-17-jre -y
sudo apt install openjdk-17-jdk -y
java --version
````
Install Apache Maven:

````
cd /opt/
````
````
wget https://dlcdn.apache.org/maven/maven-3/3.9.12/binaries/apache-maven-3.9.12-bin.tar.gz
````
````
tar -xvf apache-maven-3.9.12-bin.tar.gz
````
````
mv apache-maven-3.9.12 maven
````
````
rm apache-maven-3.9.12-bin.tar.gz
````
Configure Apache Maven Environment Variables:

````
sudo vi /etc/profile.d/mavenenv.sh
````
````
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64

export M2_HOME=/opt/maven

export PATH=${M2_HOME}/bin:${PATH}
````
````
sudo chmod 777 /etc/profile.d/mavenenv.sh
sudo chmod +x /etc/profile.d/mavenenv.sh
source /etc/profile.d/mavenenv.sh
````
Verify Maven Installation:
Check the installed version of Maven by running:
````
mvn --version
````

````
find /usr/lib/jvm/java-17* | head -n 3
````
