# Simple Jenkin & Ansible server setup into local system using docker
## initial setup
install [docker](https://docs.docker.com/engine/install/) <br>
clone the github [repository](https://github.com/Siddhartha-chaki/DockerLeanings.git)

## enviorment set up
### setup jenkins server with ansible
build the two docker images from [/DockerLeanings/docker/jenkins/Dockerfile]()
and [DockerLeanings/docker/slave/Dockerfile]()
```bash
cd DockerLeanings/docker/slave/
docker build -t jekins .
cd DockerLeanings/docker/slave/
docker build -t slv1 .
```
after building the image do 
```bash
docker image ls
```
get the container id from the ouput of docker image ls 
```bash
docker run -p 22:22 f39114bfa055 
docker run -p 8080:8080 -p 50000:50000 11d9026a836d
```
*first one for slave server to run with 22 SSH port [@for slave details refer ](https://dev.to/s1ntaxe770r/how-to-setup-ssh-within-a-docker-container-i5i)<br>
**second one for jenkins server.

### setup slave server with SSH
once both the containter up, next step to setup SSH key-pair for login, using which we can connnect slave from the jenkins.<br>
for [public and private key generation](https://docs.github.com/en/enterprise-server@2.21/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) you can reger this link.  
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
you can generate this key inside the slave server or in your local, in any case the primary gole is to put/append your public key in <b>.ssh/authorized_keys</b> and private key value in the jenkins credentials.
if we take approch of login into the container itslef, we follow as like,<br>
after building the image do 
```bash
docker container ls
```
get the container id from the above command 
```bash
docker container exec -it -u root f92472882aca /bin/bash
or above to login using roor & below to loing using default
docker container exec -it f92472882aca /bin/bash
```
once you login into slave, create the key, append the public key into the b>.ssh/authorized_keys</b> (if the file not present you can create the file manually with same name) and cat the private key file and copy the private key for later use.<br>
for private key checking you can paste the private key value into your local system, then
```bash
ssh -i slave1_prv_key.txt test@172.17.0.3
```
<br>In case of local key generation, you can do ssh-key-copy tool to tansfer your generated local key to slave. (use google its very simple)

### setup jenkins & ansible 
#### jenkins setps
once the slave part is done, now its jenkins time, hit localhost:8080 in browser, and login into jenkins container into CLI using <b>docker container exec -it "container id" /bin/bash</b> get key file value for jenkins. Set up your admin accout, install default pugins.
#### ansible setup
 Ansible already installed into container, you can do <b>$ which ansible</b> to get the install location. Once the homepage in jenkins opens up, instll the ansible plugin into jenkins, the in global tool configuration add the ansible. <br> Congratulation all the set is preaty much done
### pipeline and ansible playbook invokation
  create one pipline (new item), then into that pipeline into the credentials you can put your private key value to the use the salve in the jenkins with user id and the ip of the slave.<br>
  In the pipeline script you can refer the <b>/DockerLeanings/Jenkinsfile </b>, metion the inventory into <b>/DockerLeanings/inventory.ini </b> and ansible playbook from <b>/DockerLeanings/playbook.yml </b>
  in this example the playbook is simply doing the copy the b>/DockerLeanings/deploy/index.html </b> into the slave server. 

  <br>This tutorial is just to simple use case in mind, where ones requirement is to build the project into jenkins the tanserfer the output generated solution to the slave ex in java project, jenkins will build the jar file and ansible will tansfer the jar file to any standalone server which can be used for undreline slave system.


