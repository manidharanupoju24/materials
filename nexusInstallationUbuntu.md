### Installation on nexus repo on ubuntu

1) Create a t3.medium (minimum specification) instance with ubuntu in AWS.
2) Open port 22 in security group for SSH
3) Install Java 8 in the instance you just created (Java 8 is the minimum requirement)
```
sudo apt-get update
sudo apt-get install openjdk-8-jdk
```
4) Install nexus in /opt mount
```
cd /opt
sudo wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz
```
5) Untar the package
```
tar -zxvf latest-unix.tar.gz
```
6) Nexus folder contains the binaries and runtime for the application nexus
7) sonatype-work contains the own configuration for nexus and data (configuration files)
    * contains the directories for what you configured in nexus itself (like plugins and installations)
    * contains ip addresses that access nexus
    * logs of the nexus app itself 
    * contains your uploaded files and metadata
    * you can use this folder to backup nexus data (since it contains all the data, storage, configs, plugins that you do in nexus app)
8) Starting nexus 
    * Services should not run with root user permissions for security concerns.
    * Best practice : Create a new user for the nexus service. This user should only have the permission for that specific service.
```
sudo adduser nexus
```
    c) Run the above command and give password to nexus user
    d) Check the permissions of nexus directories, and they'll show root user as owner and group (not recommended)
    e) Change the directory permissions to nexus user that you just created (Make sure that you change nexus version according to your version)
```
chown -R nexus:nexus sonatype-work
chown -R nexus:nexus nexus-3.45.0-01
ls -l
```
    f) Set the configuration so that it runs as nexus user
```
vi nexus-3.45.0-01/bin/nexus.rc
```
    g) Uncomment "run_as_user" and add nexus user there. For e.g.
```
run_as_user="nexus"
```
    h) Switch user to nexus
```
su - nexus
```
    i) Start the service and check if the service is running
```
/opt/nexus-3.45.0-01/bin/nexus start
ps aux | grep nexus
netstat -nlpt
```
    j) You'll see that the service will be accessible on port 8081
9) Configure the security group to allow 8081 and try to access using ip address of EC2 instance with the port 8081