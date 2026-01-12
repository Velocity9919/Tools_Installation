# installing jenkins on ubunru
````
sudo hostnamectl set-hostname Jenkins
````
````
/bin/bash
````
````
sudo apt update -y
sudo apt install openjdk-17-jre -y
sudo apt install openjdk-17-jdk -y
java --version
````
````
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
````
````
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
````
````
sudo apt-get update
sudo apt-get install jenkins -y
````
````
sudo systemctl enable jenkins
````
````
sudo systemctl start jenkins
````
````
sudo systemctl status jenkins
````
```` 
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
````


````
find /usr/lib/jvm/java-17* | head -n 3
````
````
/usr/lib/jvm/java-17-openjdk-amd64
````
````
vi ~/.bash_profile
````
````
JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64

PATH=$PATH:$HOME/bin:$JAVA_HOME/bin

export JAVA_HOME

export PATH
````
````
source ~/.bash_profile
````
````
echo $PATH
````
````
sudo update-alternatives --config java
````
