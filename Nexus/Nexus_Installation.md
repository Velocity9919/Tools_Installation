# Install Nexus3 Repository Manager with TLS
Manage components, binaries and build artifacts across your entire software supply chain.
## Prerequsites 
- Virtual Machine running Ubuntu 24.04 or newer
### Update Package Repository and Upgrade Packages

```
sudo apt update
```
### Step 1 — Installing Java

```
sudo apt install openjdk-17-jdk -y
java -version
```

## Install Nexus3 Repository Manager
At time of writing v3.49.0 is the latest version. You can check if there is a newer version available by visiting - https://help.sonatype.com/repomanager3/product-information/download

## Step 2 — Downloading Nexus

```
cd /opt
```
```
sudo curl -L -O https://download.sonatype.com/nexus/3/nexus-3.87.1-01-linux-x86_64.tar.gz
```

### Extract Nexus
```
sudo tar -xvzf nexus-3.87.1-01-linux-x86_64.tar.gz
```
```
sudo mv nexus-3.87.1-01 nexus
```
```
sudo rm nexus-3.87.1-01-linux-x86_64.tar.gz
```

### Create User to run Nexus3
```
sudo adduser nexus
```
To set no password for nexus user open the visudo file in ubuntu:
```
sudo visudo
```
Add below line into it , save and exit:

```
nexus ALL=(ALL) NOPASSWD: ALL
```
### Update and Grant Permissions to Nexus user
```
sudo chown -R nexus:nexus /opt/nexus
sudo chown -R nexus:nexus /opt/sonatype-work
```
### Change default run_as user
```
sudo vi /opt/nexus/bin/nexus.rc
```
Uncomment `#run_as_user=` and modify to set nexus as user. It should read `run_as_user=”nexus”`
```
run_as_user="nexus"
```
Open the /opt/nexus/bin/nexus.vmoptions file:

```
sudo nano /opt/nexus/bin/nexus.vmoptions
```
To Increase the nexus JVM heap size, you can modify the size as shown below:

```
-XX:MaxDirectMemorySize=2703m
-Djava.net.preferIPv4Stack=true
```

### Configure Nexus to run as a service
```
sudo nano /etc/systemd/system/nexus.service
```
```
[Unit]
Description=nexus service
After=network.target

[Service]
Type=forking
LimitNOFILE=65536
ExecStart=/opt/nexus/bin/nexus start
ExecStop=/opt/nexus/bin/nexus stop
User=nexus
Restart=on-abort

[Install]
WantedBy=multi-user.target
```
### Start and Enable Nexus
```
sudo systemctl start nexus
```
```
sudo systemctl enable nexus
```
```
sudo systemctl status nexus
```

```
ufw allow 8081/tcp
```
Use the below command to open to access Nexus repository web interface:

```
http://server_IP:8081
```

To login to Nexus, click on Sign In, default username is ``` admin ```

To find default password run the below command:
```
sudo nano /opt/sonatype-work/nexus3/admin.password
```

