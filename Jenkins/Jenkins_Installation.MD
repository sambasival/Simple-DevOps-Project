# Install Jenkins on Ubuntu 18.04 LTS
Jenkins is a self-contained Java-based program, ready to run out-of-the-box, with packages for Windows, Mac OS X and other Unix-like operating systems. As an extensible automation server, Jenkins can be used as a simple CI server or turned into the continuous delivery hub for any project.


### Prerequisites
1. Ubuntu Instance 
    sudo su -
    apt-get update
    
1. Java v1.8.x 

## Install Java
1. We will be using open java for our demo, Get the latest version from https://pkg.jenkins.io/debian-stable/
   ```sh
   apt install openjdk-8-jre-headless
   apt install openjdk-8-jdk-headless
   ```

1. Confirm Java Version and set the java home
   ```sh
   java -version
   find /usr/lib/jvm/java-1.8* | head -n 3
   cat >> /etc/environment <<EOL
   JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
   JRE_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre
   EOL
   
   source /etc/environment
   echo $JAVA_HOME
   ```
   _The output should be something like this,_
    ```sh
   [root@~]# java -version
   openjdk version "1.8.0_292"
   OpenJDK Runtime Environment (build 1.8.0_292-8u292-b10-0ubuntu1~18.04-b10)
   OpenJDK 64-Bit Server VM (build 25.292-b10, mixed mode)
   ```

## Install Jenkins
 You can install jenkins using the rpm or by setting up the repo. We will set up the repo so that we can update it easily in the future.
1. Get the latest version of jenkins from https://pkg.jenkins.io/redhat-stable/ and install
   ```sh
   sudo apt-get -y install wget
   sudo wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -   
   sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
   sudo apt update
   sudo apt install jenkins
   
   ```

   ### Start Jenkins
   ```sh
   # Start jenkins service
   sudo systemctl start jenkins
   sudo systemctl status jenkins
   
   # Setup Jenkins to start at boot,
   #chkconfig jenkins on
   ```
   ### Opening the firewall
   sudo ufw allow OpenSSH
   sudo ufw enable
   sudo ufw allow 8080
   sudo ufw status

   ### Accessing Jenkins
   By default jenkins runs at port `8080`, You can access jenkins at
   ```sh
   http://YOUR-SERVER-PUBLIC-IP:8080
   ```
  #### Configure Jenkins
- The default Username is `admin`
- Grab the default password 
- Password Location:`/var/lib/jenkins/secrets/initialAdminPassword`
- `Skip` Plugin Installation; _We can do it later_
- Change admin password
   - `Admin` > `Configure` > `Password`
- Configure `java` path
  - `Manage Jenkins` > `Global Tool Configuration` > `JDK`  
- Create another admin user id

### Test Jenkins Jobs
1. Create “new item”
1. Enter an item name – `My-First-Project`
   - Chose `Freestyle` project
1. Under the Build section
	Execute shell: echo "Welcome to Jenkins Demo"
1. Save your job 
1. Build job
1. Check "console output"
