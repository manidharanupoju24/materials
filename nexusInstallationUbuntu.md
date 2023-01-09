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
```