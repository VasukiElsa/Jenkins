# About my Project:
This project is simple jenkins pipeline that uses docker as an agent. And checks that docker agent configuration is working in the jenkins pipeline as expected.

## Architecture:
Master node : Jenkins\
Agents : Docker Container

### Inside the Jenkinsfile, it has two stages that are
1. It displays "Hello World"
2. It checks the node --version.

### Steps:
1. Create an EC2 instance on AWS then install jenkins
2. Prerequisites to install jenkins is to install java
  ``` $ sudo apt update
   $ sudo apt install openjdk-17-jre
```
3. Install Jenkins
 ```  $ curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```
4. Then add inbound rule for jenkins to access the port **8080**
5. Then sign-in to the jenkins by *"http://<public-ip-instance>:8080"*
6. Install Docker on the same EC2 instance
   ```$ sudo apt install docker.io```
7. Grant Jenkins user and ubuntu user permission to docker daemon.
  ``` $ sudo su -
   usermod -aG docker jenkins\
   usermod -aG docker ubuntu\
   systemctl restart docker```
8. Switch to jenkins user
 ```  $ su - jenkins ```
9. Jenkins user is created by default when you install the software
   ``` $ docker run hello-world```\
   This ensures docker has installed and user has access to docker.
10. Now jenkins user is also able to create the containers or run the containers.
11. Restart your jenkins\
   *(Go to your jenkins url, then /restart)*.
12. Once you install docker, the other thing you have to do is you have to install the docker plugin inside this jenkins.
13. Now Go to **'manage jenkins'**, then click **'available plugins'**, then type **"docker pipeline"**.
14. Then select **"Pipeline Project"** and add groovy code for our pipeline.
15. Then click **"Build Now"**, it executes our pipeline inside the docker agent.

### Console Output:
1. It checks docker image is already available.
   ```$ docker inspect -f . node:16-alpine```
2. If not, it grab that image from docker-hub and run the container.
3. It displays, "Hello World".
4. Then it checks image version.
5. Then it stops the container and removes them.

