### Installation of Jenkins on Ubuntu as docker container

1) Create a t2.medium ec2 instance in AWS. 
2) Open port 8080 in security groups
3) SSH into the instance something like shown below :
```
ssh -i "sample.pem" ubuntu@ec2-xx-xx-xx-xx.us-west-2.compute.amazonaws.com
```
4) If it is a ubuntu instance (ubuntu preferred), do the below: 
```
sudo su 
apt update
apt install docker.io
```
5) Test if docker is installed 
```
docker ps
```
6) Run Jenkins as docker container. Open port 8080 for exposing jenkins to browser
7) Open port 50000 for jenkins master and slave to communicate with each other
```
sudo docker run -p 8080:8080 -p 50000:50000 -d -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
```
8) Check if the jenkins is running as a docker container
```
docker ps
```
9) Use the public ip along with port 8080 and check in the browser, if you are able to see the jenkins home page
10) For initial admin password, you can do this :
```
sudo docker exec -it 31b732231da6 bash
# you will be inside the container's bash : 
cat /var/jenkins_home/secrets/initialAdminPassword
```
11) You can check the same password in your jenkins linux server
```
docker volume inspect jenkins_home
```
12) Use this password and login. Install all the recommended plugins and you will be able to see the jenkins home page.

### Building a Docker Image from the Jar file

13) To make docker runtime available in jenkins container : 
```
docker run -p 8080:8080 -p 50000:50000 -d -v jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):/usr/bin/docker jenkins/jenkins:lts
```

14) Now docker would be available in the jenkins container, but you won't be able to run the docker commands as the jenkins user will not have permissions. 

15) You can enter the container as thr root user and add permissions to jenkins user.
```
docker exec -u 0 -it f0417b0a332e bash
```
Note : Make sure you change the container id, according to your requirement.

16) Give permissions to jenkins user to run docker after you login as root. 
```
chmod 666 /var/run/docker.sock
```

17) Now login as jenkins user again and check if you can run docker commands 
```
docker exec -it f0417b0a332e bash
docker pull redis
docker ps
```

18) You should be able to run and this basically means that you can execute docker commands from Jenkins UI client, in job as a shell command.

19) Now in your Jenkins job, add a build step to run shell command and run :
```
docker build -t test-image-name .
```

20) The above step will help you build a local docker image in jenkins server. 