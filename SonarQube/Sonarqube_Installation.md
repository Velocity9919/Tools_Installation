# How to Install Sonarqube in Ubuntu Linux
SonarQube is an open-source platform developed by SonarSource for continuous inspection of code quality to perform automatic reviews with static analysis of code to detect bugs and code smells on 29 programming languages.
## Prerequsites 
Install SonarQube on Ubuntu 24.04 LTS

AWS Account with Ubuntu 24.04 LTS EC2 Instance.
At least 2 CPU cores and 4 GB of RAM for smooth performance.

### Update Package Repository and Upgrade Packages

```
sudo apt update
```
```
sudo apt install openjdk-17-jdk -y
```
```
java --version
```
### Step #2:Configure System Limits
```
sudo vi /etc/sysctl.conf
```
Add the following at the end.
```
vm.max_map_count=262144
fs.file-max=65536
```
```
sudo sysctl --system
```
### Step #3:Configure Limits for the User
```
sudo vi /etc/security/limits.conf
```
Add these lines at the end.
```
sonarqube - nofile 65536
sonarqube - nproc 4096
```
### Step #4:Install PostgreSQL and Configure the Database

```
sudo apt install postgresql postgresql-contrib -y
```
```
sudo systemctl start postgresql
sudo systemctl enable postgresql
```
```
sudo systemctl status postgresql
```
Switch to PostgreSQL user.

```
sudo -i -u postgres
```
Open the PostgreSQL shell.

```
psql
```
Create a new PostgreSQL user named sonarqube and sets its password to sonar123.
```
CREATE USER sonarqube WITH ENCRYPTED PASSWORD 'sonar123';
```
Create a new database named sonarqube and assigns its ownership to the sonarqube user we just created.
```
CREATE DATABASE sonarqube OWNER sonarqube;
```
Grant all available privileges on the sonarqube database to the sonarqube user.
```
GRANT ALL PRIVILEGES ON DATABASE sonarqube TO sonarqube;
```
Revoke all default permissions on the public schema from the public role.
```
REVOKE ALL ON SCHEMA public FROM public;
```
Grant the sonarqube user ALL privileges on the public schema within the sonarqube database.
```
GRANT ALL ON SCHEMA public TO sonarqube;
```
Connect to the newly created sonarqube database
```
\c sonarqube;
```
Set default privileges for any future tables, sequences, and functions created by the sonarqube user within the public schema.
```
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL ON TABLES TO sonarqube;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL ON SEQUENCES TO sonarqube;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL ON FUNCTIONS TO sonarqube;
```
Exit the PostgreSQL command-line client.
```
\q
```
Exit the postgres user shell, returning to regular Ubuntu user.
```
exit
```
### Step #5:Install and Configure SonarQube
```
sudo adduser --system --no-create-home --group sonarqube
```
```
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-25.6.0.109173.zip
```
```
sudo apt install unzip -y
```
```
sudo unzip sonarqube-25.6.0.109173.zip -d /opt/
```
```
sudo mv /opt/sonarqube-25.6.0.109173 /opt/sonarqube
```
1️⃣ SonarQube must NOT run as root

```
sudo useradd sonar
sudo chown -R sonar:sonar /opt/sonarqube
```
3️⃣ Elasticsearch system limits (mandatory)
```
sudo sysctl -w vm.max_map_count=262144
```

```
sudo vi /opt/SonarQube/conf/sonar.properties
```
Uncomment and set these properties. This connects SonarQube to PostgreSQL and makes it accessible from any IP.

```
#--- DATABASE
sonar.jdbc.username=sonarqube
sonar.jdbc.password=sonar123

#--- PostgreSQL 13 or greater
sonar.jdbc.url=jdbc:postgresql://localhost/sonarqube

#--- WEB SERVER
sonar.web.host=0.0.0.0
sonar.web.port=9000
```

### Create a new systemd service file.

```
sudo nano /etc/systemd/system/sonarqube.service
```
```
[Unit]
Description=SonarQube service
After=network.target

[Service]
Type=forking

ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop

User=sonar
Group=sonar
Restart=always
LimitNOFILE=65536
LimitNPROC=4096

[Install]
WantedBy=multi-user.target
```
### Reload systemd. Start and enable the service.

```
sudo systemctl daemon-reload
sudo systemctl enable sonarqube
sudo systemctl start sonarqube
```
```
sudo systemctl status sonarqube
```
### Step #6:Access SonarQube Web Interface

http://<Your-EC2-Public-IP>:9000

Username: admin

Password: admin




Nginx should now be serving your domain name. You can test this by navigating to https://your_domain
