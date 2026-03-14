# installing jenkins on ubunru
````
sudo hostnamectl set-hostname Jenkins
````
````
/bin/bash
````
````
sudo apt update -y
sudo apt upgrade -y
sudo apt install fontconfig openjdk-21-jre
java -version
````
````
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins
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
find /usr/lib/jvm/java-21* | head -n 3
````
````
/usr/lib/jvm/java-21-openjdk-amd64
````
````
vi ~/.bash_profile
````
````
JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64

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
