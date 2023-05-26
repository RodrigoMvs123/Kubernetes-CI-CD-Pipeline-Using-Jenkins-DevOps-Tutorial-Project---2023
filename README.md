# Kubernetes-CI-CD-Pipeline-Using-Jenkins-DevOps-Tutorial-Project---2023

https://www.youtube.com/watch?v=q4g7KJdFSn0

https://raw.githubusercontent.com/RodrigoMvs123/Kubernetes-CI-CD-Pipeline-Using-Jenkins-DevOps-Tutorial-Project---2023/main/README.md


Application Repository 
https://github.com/dmancloud/complete-prodcution-e2e-pipeline

GitOps Repository 
https://github.com/dmancloud/gitops-complete-prodcution-e2e-pipeline 

The Pipeline We Are Building 
Developer ( Java Application ) 
Push Code ( Github - Application Repository )
Notify Jenkins ( 192.168.1.10 - There is a new application update ) 
Build and Test ( Jenkins )
Maven  ( 192.168.1.11 - Build the application to perform some tests ) 
Sonarqube ( 192.168.1.12 - Static Code Analysis - Potential problems and Security before push into production ) 
Build and Push Docker Image ( Into to Docker Hub Registry ) 
Trivy Artifact Scan ( Scan using Static Analysis - Container - Security check )
Update Kubernetes Manifests ( Separated Repositories )  
DevOps Repository ( Github - Deployment Manifest - Services Manifest )
Tag ( 192.168.1.13 - Last Version of the Tag - Tag Updated ) Tag detected 
Kubernetes Cluster ( Deploy the last version of the Tag ) 
Send Notification ( Deployment and build are successful or any problems along the way ) 

Declarative Pipeline ( Jenkins ) 
Individual Virtual Machine 
DNS entries ( For each of the IP Addresses ) Reverse Proxies - TLS Certificates    
Sign into to the first virtual machine ( Jenkins UI Reside ) Become Root 

Docker 
Update Package Repository and Upgrade Packages 
> ssh 192.168.1.10
> ssh sudo -i 

Run from shell prompt 
sudo apt update
sudo apt upgrade 

root@jenkins-ui:~# sudo apt update
sudo apt upgrade 

Docker 
Install Java 
Add Adoptium Repository ( Copy and Past ) 
wget -0 - https://packages.adoptium.net/artifactory/api/gpg/key/public | tee /etc/apt/keyrings/adoptium.asc
echo “deb [ signed-by=etc/apt/keyrings/adoptium.esc ] https://packages.adoptium.net/artifactory/deb $(awk -F= ‘/^VERSION_CODENAME/{print$2}’ /etc/os-release) main” tee /etc/apt/source.list./adoptium.list

root@jenkins-ui:~# wget -0 - https://packages.adoptium.net/artifactory/api/gpg/key/public | tee /etc/apt/keyrings/adoptium.asc
echo “deb [ signed-by=etc/apt/keyrings/adoptium.esc ] https://packages.adoptium.net/artifactory/deb $(awk -F= ‘/^VERSION_CODENAME/{print$2}’ /etc/os-release) main” tee /etc/apt/source.list./adoptium.list

Docker
Update Repository and Install Java 
apt update 
apt install temurin-17-jdk 

root@jenkins-ui:~# apt update 
apt install temurin-17-jdk

Docker
Install Jenkins 

Run from shell prompt 
curl -fsSL https://pkg.jenkins.io …
sudo apt-get install jenkins 

root@jenkins-ui:~# curl -fsSL https://pkg.jenkins.io … 

root@jenkins-ui:~# sudo apt-get install jenkins 

Docker
Starting Jenkins 

Run from shell prompt 
sudo systemctl start jenkins 

root@jenkins-ui:~# sudo systemctl start jenkins 

Docker
Run from shell prompt

root@jenkins-ui:~# sudo systemctl status jenkins 

192.168.1.10:8080
Getting Started ( Unlock Jenkins ) 
/var/lib/jenkins/secrets/initialAdminPassword
Administrator Password: c35873ec360a4b7cb4593384a646e2449

root@jenkins-ui:~# cat /var/lib/jenkins/secrets/initialAdminPassword
c35873ec360a4b7cb4593384a646e2449

Customize Jenkins 
Install Suggested Plugins 
( Getting Install ) 

Install nginx Reverse Proxy for Jenkins 

Docker
Installing Nginx 
sudo apt install nginx 

root@jenkins-ui:~# sudo apt install nginx  

root@jenkins-ui:~# systemctl status nginx 

Access the Application on Port 8080 
192.168.1.10:8080

192.168.1.10 ( Welcome to nginx ) 

Docker
Run from shell ( Replace your domain ) 
sudo vi etc/nginx/sites-available/jenkins.dev.dman.cloud


root@jenkins-ui:~# sudo vi etc/nginx/sites-available/jenkins.dev.dman.cloud

Docker
Past in the following configuration block, which is similar to the default, but updated for our directory and domain name:

Place the below ( Replace your domain )  

Upstream jenkins {
       server 127.0.0.1:8080;
}

server {
       listen              80;
       server_name jenkins.dev.dman.cloud;

       access_log    /var/log/nginx/jenkins.access.log;
       error_log       /var/log/nginx/jenkins.error.log;

      proxy_buffers 16 64k;
      proxy_buffers_size 128k;

      location / {
             proxy_pass https://jenkins;
             proxy_next-upstream error timeout invaid_header http_500 http_502 http_503 http_504 
             proxy_redirect off: 

             proxy_set_header       Host                     $host;
             proxy_set_header       X-Real-IP             $remote_addr;
             proxy_set_header       X-Forwarded-For $proxy_add_x_forwarded_for;
             proxy_set_header       X-Forwarded-Proto https;
       }
}

Docker
Next, let´s enable the file by creating a link from it to the site-enabled directory, which nginx  reads from during startup: 

Run from shell prompt ( replace your domain )
sudo ln -s /nginx/etc/sites-available/jenkins.dev.dman.cloud /etc/nginx/sites-enabled/

root@jenkins-ui:~# sudo ln -s /nginx/etc/sites-available/jenkins.dev.dman.cloud /etc/nginx/sites-enabled/

root@jenkins-ui:~# nginx -t

Docker
If there are not any problems, restart nginx to enable your changes:
Run from prompt
sudo systemctl restart nginx  

root@jenkins-ui:~# sudo systemctl restart nginx  

192.168.1.10 ( nginx ) 

https://jenkins.dev.dman.cloud 
Getting Started ( Unlock Jenkins ) 
/var/lib/jenkins/secrets/initialAdminPassword
Administrator Password: c35873ec360a4b7cb4593384a646e2449

192.169.1.10:8080 
Create First Admin User 

Username - admin 
Password - …
Confirm password - …
Full name - Dinesh Mistry 
Email address - dinesh@dman.cloud 

Instance Configuration
Jenkins URL: https//jenkins.dev.dman.cloud/ (192.168.1.10:8080)
Jenkins is ready !
Your jenkins setup is complete. 
Start using Jenkins 

Add Certificate to secure the installation 
Configure Jenkins with TLS

Docker
Update Package Repository and Upgrade Packages
Installing Certbot
The first step to using Let´s Encrypt to obtain an SSL certificate is to install the Certbot software on your server.
Run from promt 
sudo apt install certbot phyton3-certbot-nginx 
root@jenkins-ui:~# sudo apt install certbot phyton3-certbot-nginx 

Docker
Obtain an SLL Certificate 
Certbot provides a variety of ways to obtain SSl Certificates through plugins. The Nginx plugin will take care of reconfiguring and reloading the config wherever necessary. To use this plugin type the following:
sudo certbot - -nginx -d jenkins.dev.dman.cloud 

root@jenkins-ui:~# sudo certbot - -nginx -d jenkins.dev.dman.cloud 
( Enter ‘c’ to cancel ) dinesh@dman.cloud 
(Y)es/(N)o: Y
(Y)es/(n)o: N 

Valid Certificate - Padlock  https://jenkins.dev.dman.cloud 

Welcome to Jenkins!
admin 
…
Keep me signed in
Sign in 

jenkins.dev.dman.cloud 

Docker
Separate Virtual Machine ( Configure using SSH )

Adding an SSH Based Agent with Docker to Jenkins 

root@jenkins-ui:~# logout 
> # ~
Connection to 192.168.1.10 closed 

> # ~ clear 

> # ~ ssh 192.168.1.11 ( Another Virtual Machine ) Will act as our Agent < with wistry@jenkins-agent < at 07:29:18 AM

> # ~ sudo -i 

Docker
Update Package Repository and Upgrade Package 
Run from shell prompt 
sudo apt update 
sudo apt upgrade 

root@jenkins-agent:~# sudo apt update 
sudo apt upgrade 

Docker
Create Jenkins User 
sudo adduser jenkins 
root@jenkins-agent:~# sudo adduser jenkins 
New Password: …
Retype new password: …

Enter the new value or press ENTER for the default 
full name []: Jenkins Agent
Room Number []: 
Work Phone []: 
Home Phone []: 
Other []: 

Docker
Create Jenkins User 
Grant Sudo Right to Jenkins User
Run from shell prompt 
sudo usermod -aG sudo jenkins 

root@jenkins-agent:~# sudo usermod -aG sudo jenkins 

Docker
Add Adoptium Repository 
wget -0 - https://packages.adoptium.net/artifactory/api/gpg/key/public | tee /etc/apt/keyrings/adoptium.asc
echo “deb [ signed-by=etc/apt/keyrings/adoptium.esc ] https://packages.adoptium.net/artifactory/deb $(awk -F= ‘/^VERSION_CODENAME/{print$2}’ /etc/os-release) main” tee /etc/apt/source.list./adoptium.list

root@jenkins-agent:~# apt install get

root@jenkins-agent:~# wget -0 - https://packages.adoptium.net/artifactory/api/gpg/key/public | tee /etc/apt/keyrings/adoptium.asc
echo “deb [ signed-by=etc/apt/keyrings/adoptium.esc ] https://packages.adoptium.net/artifactory/deb $(awk -F= ‘/^VERSION_CODENAME/{print$2}’ /etc/os-release) main” tee /etc/apt/source.list./adoptium.list

Docker
Install Java 11 
apt update
apt install temurin-11-jdk

root@jenkins-agent:~# apt update
root@jenkins-agent:~# apt install temurin-17-jdk ( Java 17 ) 
Do you want to continue [Y/n] Y

Docker
Install Docker on Debian 11 
Install using the repository 
Before you install Docker Engine for the first time on a new host machine, you need to set up the docker repository. Afterward, you can install and update Docker from the repository.
Set up the Repository
Update the apt package index and install packages to allow apt to use a Repository over HTTPS:
sudo apt-get update
sudo apt-get install \
ca-certificates \
curl \ 
grupg

root@jenkins-agent:~# sudo apt-get update
sudo apt-get install \
ca-certificates \
curl \ 
grupg

Docker
Add Docker´s official GPG key:
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg - -dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

root@jenkins-agent:~# sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg - -dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

Docker
Use the following command to set up the repository 
$ echo \
“deb [arch=’$(dpkg - -print-architecture)” signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian\
“$(. /etc/os-release && echo “$VERSION_CODENAME”)” stable´ | \ 
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

root@jenkins-agent:~# $ echo \
“deb [arch=’$(dpkg - -print-architecture)” signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian\
“$(. /etc/os-release && echo “$VERSION_CODENAME”)” stable´ | \ 
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

Docker
Install Docker Engine This procedure works for Debian on x86_64/amd64,armhf,arm64, and Raspbian.
Update the apt package index:
$ sudo apt-get update

root@jenkins-agent:~# $ sudo apt-get update

Docker
Install Docker Engine, containerd, and Docker Compose.
Latest
To install the latest version, run:
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin 

root@jenkins-agent:~# $ $ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin 

Docker
To give privileges to the jenkins user 
Add Jenkins User to the Docker Root

Next Steps 
Continue to Post-installation steps for Linux 
To create the docker group and add your user:
Create the docker group.
$ sudo groupadd docker 

root@jenkins-agent:~# $ sudo groupadd docker
Docker
Add your user to the docker group
$ sudo usermod -aG docker $USER 

root@jenkins-agent:~# $ sudo usermod -aG docker jenkins 

Docker
You can also run the following command to activate the changes to groups 
$ newgpr docker 

root@jenkins-agent:~# su - jenkins ( Root - User Jenkins ) 

root@jenkins-agent:~# docker run hello-world 

root@jenkins-agent:~# exit
root@jenkins-agent:~# logout 
> # ~ 
connection to 192.168.1.11 closed.
root@jenkins-agent:~# clear

Docker 
Connect to Remote SHH Agent ( Virtual Machines to communicate via remote SSH Agent )
From the jenkins UI ( Controller ) 
Run from shell prompt 
ssh jenkins@$AGENT_HOSTNAME 

Terminal 1 
> # ~ 
> ssh 192.168.1.10 ( Sign in to Jenkins-ui ) 
> # ~                                                                   < with dmstry@jenkins-ui < at 07:33:56 AM
> ssh jenkins@192.168.1.11 ( Sign into our machine ) 
Are you sure you want to continue connecting (yes/no[fingerprint])? yes 
jenkins@192.168.1.11´s password: …
root@jenkins-agent:~$ 

Docker 
Connect to Remote SSH Agent 
Create private and public SSH key. The following command creates the private key jenkinsAgent_rsa and the public key jenkinsAgent.rsa.pub. It is reconnected to store your keys under ~/.ssh/ so we move to that directory before creating the key pair.
Run from shell prompt 
mkdir ~/.ssh; cd ~/.ssh/ && ssh-keygen -t rsa -m PEM -C “ Jenkins agent key “ -f “jenkinsAgent-rsa”   

root@jenkins-agent:~$ mkdir ~/.ssh; cd ~/.ssh/ && ssh-keygen -t rsa -m PEM -C “ Jenkins agent key “ -f “jenkinsAgent-rsa”   

Docker
Ensure that the permissions of the ~/.ssh directory is secure, as most ssh daemons will refuse to use keys that have file permissions that are considered insecure:
Run from shell prompt
chmod 700 ~/.ssh ( Change permissions to make more secure ) 
chmod 600 ~/. ssh/authorized_keys ~/.ssh jenkinsAgent_rsa 

root@jenkins-agent:~/.ssh$ chmod 700 ~/.ssh
chmod 600 ~/. ssh/authorized_keys ~/.ssh jenkinsAgent_rsa 

root@jenkins-agent:~/.ssh$ ls -al

root@jenkins-agent:~/.ssh$ docker run hello-world^C

root@jenkins-agent:~/.ssh$ chmod 600 ~/. ssh/authorized_keys ~/.ssh jenkinsAgent_rsa* 

root@jenkins-agent:~/.ssh$ chmod 600 ~/. ssh/authorized_keys ~/.ssh jenkinsAgent_rsa 

root@jenkins-agent:~/.ssh$ ls -al

root@jenkins-agent:~/.ssh$ clear 

Docker 
Copy the private SSH key (~/.ssh/jenkinsAgent_rsa) from the agent machine to your OS clipboard 
Run from promt 
cat ~/.ssh/jenkinsAgent_rsa 

root@jenkins-agent:~/.ssh$ cat ~/.ssh/jenkinsAgent_rsa 

root@jenkins-agent:~/.ssh$  ssh jenkins@192.168.1.10 ( User interface Virtual Machine ) 
Are you sure you want to continue connecting (yes/no[fingerprint])? yes 


Terminal 2 
( Jenkins Agent ) 
Last login: Fri Apr 14 09:47:24 from 192.168.1.164 
> # ~ ssh 192.168.1.10
> # ~ passwd jenkins                                        < with dmstry@jenkins-ui < at 07:36:36 AM
> # ~ sudo passwd jenkins 
New password: …
Retype new password: …                      < took 4s < with dmstry@jenkins-ui < at 07:36:50 AM 

Jenkins UI 
Dashboard
Manage Jenkins 
System Configuration 
Manage Nodes and Clouds ( Add,remove, control and monitor the various nodes tha Jenkins runs jobs on.
( To Disable Runs on )
Manage Nodes and Clouds
Name 
Build-in Node 
Status 
Configure
Number of executors ( 0 ) - Save 
Dashboard > Manage Jenkins > Nodes 
New Node
Node Name
jenkins-agent 
Type ( Permanent Agent ) - Enable 
Create 
Number of executors ( 2 ) 
Remote root directory ( /home/jenkins ) 
Labels ( jenkins-agent )
Launch method ( Launch agents via SSH )  
Host ( 192.168.1.11 )
Credentials ( Add ) ( Jenkins ) 
Jenkins Credentials Provider: Jenkins 
Add Credentials 
Kind ( SSH Username with private key ) ( Add ) 
ID ( jenkins-agent-key )
Description ( jenkins-agent-key )
Username ( jenkins )
Private Key ( Enter directly ) ( Add ) Enter new secret bellow ( Copy and Past )
BEGIN RSA PRIVATE KEY
MIIG4 …
END RSA PRIVATE KEY 
Add
Credentials ( jenkins (jenkins-agent-key)) 
Save 

Manage Nodes and Clouds
Name 
jenkins-agent 
Agent jenkins-agent ( This node is being launched )
Log
No known hosts files was found at ( /var/lib/jenkins/ - Copy and Past ) 
 
Terminal 1 
root@jenkins-ui:~$ clear 
root@jenkins-agent:~$ cd /var/lib/jenkins/
root@jenkins-agent:~$ ls -al 
root@jenkins-agent:~$ ssh jenkins@192.168.1.11 
Are you sure you want to continue connecting (yes/no[fingerprint])? yes 
jenkins@192.168.1.11´s password: … ( node host file )
jenkins@jenkins-agent:~$ logout 
Connection to 192.168.1.11 closed.
jenkins@jenkins-ui:~$ clear

jenkins@jenkins-ui:~$ ls -al
jenkins@jenkins-ui:~$ cd .ssh/
jenkins@jenkins-ui:~/.ssh$ ls -al
jenkins@jenkins-ui:~/.ssh$ cat known hosts ( It has been created ) 

Jenkins UI 
Configure
Save
Relaunch agent 

The SSH key presented by the remote host does not match the key saved in the known host file against this host.

jenkins@jenkins-ui:~/.ssh$ clear

jenkins@jenkins-ui:~/.ssh$ cat known_hosts 

Terminal 2 
ssh jenkins@192.168.1.11
ssh jenkins@192.168.1.11´s password: …
jenkins@jankins-agent:~/$ 

Docker 
Add the public SSH key to the list of authorized on the agent machine
Run from prompt Shell
cat jenkinsAgent_rsa.pub > > ~/.ssh/authorized_keys

Terminal 2 
jenkins@jankins-agent:~/$ clear
jenkins@jankins-agent:~/$ cd 
jenkins@jankins-agent:~/$ cd .ssh/
jenkins@jankins-agent:~/.ssh$ ls -al  
jenkins@jankins-agent:~/.ssh$ cat jenkinsAgent_rsa.pub > > ~/.ssh/authorized_keys

Jenkins UI 
Nodes ( Manage Nodes and Clouds )
Name
jenkins-agent
Launch agent

Dashboard 
New Item 
Enter an item name 
test
Pipeline ( Job ) 
Ok

Advanced Project Options 
Pipeline 
Pipeline script 
Script ?
                                          Try sample Pipeline ( Hello-World ) 
pipeline {
       any agent 
       stages {
              stage ( ‘ Hello ‘ ) {
                    steps {
                           echo ‘Hello World’
                    }
              }
        }
}

Save 

Build Now
Pipeline Test 

Stage View   
                                           Hello 
       Average stage times:  124ms 
(Average full run time: ~2s)
#1 
   Apr 14 No Changes         124ms
   10:44

Dashboard
Name 
Test ( Delete Pipeline ) 

( Configure our pipeline with the complete DevOps pipeline )
( Created Application - make the repositories public after this demonstration ) 
( Basic Web Page in Java )

Shell Terminal 1 

jenkins@jankins-agent:~/.ssh$ logout 
Connection to 192.168.1.10 closed.

> # ~ clear

( Back to our Local Machine )
> # ~ cd ~/Projects/dmancloud/
> cd ~/Projects/dmancloud
> ls
complete-production-e2e-pipeline      gitops-complete-production-e2e-pipeline
> # ~ cd ~/Projects/dmancloud
> # ~ Projects/dmancloud    complete-production-e2e-pipeline/
> cd complete-production-e2e-pipeline
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !3 ?1 mvn clean install( Pipeline production ) 

…
> ls
( Run the application to local machine - Application Repository 
https://github.com/dmancloud/complete-prodcution-e2e-pipeline ) 
LICENSE READM.md mvnw mvnw.cmd pom.xml src target 
> mvn clean install 
 
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !4 java -jar target/demoapp.jar 

127.0.0.1:8080 
( The Web Page Application ) 
 
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !4 clear

…
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !4 code.
Microsoft Visual Studio Code
New File
Jenkinsfile ( set up initial pipeline )
       pipeline{
              agent 
                     label “jenkins-agent”
              }
              stages{ 
                     stage(“A”){
                            steps{  
                                   echo “=======executing A=======”
                            }
                     }
               }
       }

Install Java Maven Tools 

Jenkins UI
Dashboard
Manage Jenkins 
Manage Plugins ( Add,remove,disable or enable plugins that can extend the functionality of Jenkins )
Plugins 
Maven ( Maven Integration and Pipeline Maven Integration )
Download now and install after restart
Adoptium ( Eclipse Temurin installer ) 
Download now and install after restart

Restart Jenkins when installation is complete and jobs are running.

Visual Studio Code
Jenkinsfile ( set up initial pipeline ) - https://github.com/dmancloud/complete-prodcution-e2e-pipeline
       pipeline{
              agent 
                     label “jenkins-agent”
              tools{
                     jdk ‘java17’
                     maven ‘Maven3’
              }
              stages{ 
                     stage(“Cleanup Workspace”){
                            steps{  
                                   cleanWs{} ( Build Function that comes with Jenkins ) 
                            }
                     }
               }
              stages{ 
                     stage(“Checkout from SCM”){
                            steps{  
                                 git branch: ‘main’, credentialsId: ‘github’, url: ‘https://github.com/dmancloud/complete-prodcution-e2e-pipeline
’
                            }
                     }
               }
       }

jenkins.dev.dman.cloud 

Welcome to Jenkins
admin
…
Keep me sign in 
Sign in 

Jenkins UI

Configure the plugins installed 
Manage Jenkins 
Global Tool Configuration (Configure tools, their locations and automatic installers)
Global Tool Configuration 
Maven
Maven installations 
List of Maven installations on this system
Add Maven 
Name - Maven3 ( Saved on the pipeline ) 
Install from Apache ( Version ) - 3.9.1
Applay
Save
Global Tool Configuration (Configure tools, their locations and automatic installers)
Configure Java ( Search jdk ) 
JDK 
JKD Installations 
List of JDK installations on this system 
Add JDK
Name - Java17 ( Saved on the pipeline ) 
Install Automatically 
Add Installer 
Install from adoptium.net
Version - jdk-17.0.5+8
Apply
Save

Make sure if github credentials are set up.

Jenkins UI

Manage Credentials ( Configure Credentials ) 
Stores Scoped to Jenkins 
Store
System - ( Global / Add new credentials ) 
New Credentials 
Kind
Username with password 
Username- dmancloud ( Personal Access Token / Created on Github ) 
Github Personal Access Token - ghp_tt5kWEH6ZuLqzO7GuUNQQNWEHQFF63pj0OT
Password: … (ghp_tt5kWEH6ZuLqzO7GuUNQQNWEHQFF63pj0OT)
ID - github ( Saved on the pipeline ) 
Description - github
Create
Dashboard 

Set up national Pipeline 
New Item ( Pipeline ) 
Create 
Enter an item name

> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !4 pwd

Enter an Item name 
complete-production-e2e-pipeline ( Application Name ) 
Pipeline ( Workflows )
Ok
General
Discard old builds
Max # of builds to keep - 2
Advance Project Options
Definition
Pipeline - Pipeline script from SCM ( Use pipeline from our repository ) 
SCM - Git
Repositories 
Repository URL - https://github.com/dmancloud/complete-prodcution-e2e-pipeline
Credentials dmancloud/…(github) - …Personal Access Token 
Add Repository 
Branches to build ? - /main
Apply 
Save

Shell Terminal 2
Commit into our repository 
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !4 git add .
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !4 git commit -m “added Jenkinsfile”
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main ->1 git push origin main
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main 

Visual Studio Code
Jenkins UI 
Pipeline complete-production-e2e-pipeline
Build now - #1 Apr 14.2023.8:09 AM - Console Output 

Jenkinsfile    
pipeline{
              agent 
                     label “jenkins-agent”
              tools{
                     jdk ‘java17’
                     maven ‘Maven3’
              }
              stages{ 
                     stage(“Cleanup Workspace”){
                            steps{  
                                   cleanWs{} ( Build Function that comes with Jenkins ) 
                            }
                     }
      
                     stage(“Checkout from SCM”){
                            steps{  
                                 git branch: ‘main’, credentialsId: ‘github’, url: ‘https://github.com/dmancloud/complete-prodcution-e2e-pipeline
’
                            }
                     }
               }
       }

Save 

Shell Terminal 2
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main git add .
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main ->1 git commit -m “fixed steps in Jenkinsfile”
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main ->1 git push origin main

Jenkins UI
Pipeline complete-production-e2e-pipeline
Build now - #2 Apr 14.2023.8:11 AM - Console Output 

Pipeline complete-production-e2e-pipeline
Stage View 

                                   Declarative   Declarative:   Cleanup       Checkout
                                   Checkout      Tool Install     Workspace  from SCM
                                   SCM  
Average stage times  6s                  40s                373ms         3s
#2
Apr 14   No Changes 6s                  40s                373ms         3s 
11:11
#1
Apr 14   No Changes 
11:09

Build the Application itself ( And execute some tests )
To create two more stages ( For build and Test )

Visual Studio Code
Jenkinsfile - https://github.com/dmancloud/complete-prodcution-e2e-pipeline
pipeline{
              agent 
                     label “jenkins-agent”
              tools{
                     jdk ‘java17’
                     maven ‘Maven3’
              }
              stages{ 
                     stage(“Cleanup Workspace”){
                            steps{  
                                   cleanWs{} ( Build Function that comes with Jenkins ) 
                            }
                     }
      
                     stage(“Checkout from SCM”){
                            steps{  
                                 git branch: ‘main’, credentialsId: ‘github’, url: ‘https://github.com/dmancloud/complete-prodcution-e2e-pipeline
’
                            }
                     }

                     stage(“Build Application”){
                            steps{  
                                   sh “mvn clean package”   
                            }
                     }

                    stage(“Test Application”){
                            steps{  
                                   sh “mvn test ”    
                            }
                     }
               }
       }

Shell Terminal 2
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main git add .
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main git commit -m “added build and test stage 
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main ->1 git push origin main 
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main

Jenkins UI
Pipeline complete-production-e2e-pipeline
Build now - #1 Apr 14.2023.8:14 AM - Console Output 

Pipeline complete-production-e2e-pipeline
Stage View 


                                   Declarative   Declarative:   Cleanup       Checkout   Build           Test
                                   Checkout      Tool Install     Workspace  from SCM  Application  Application
                                   SCM  
#3 
Apr 14
11:14 No Changes     1s 
Average stage times  6s                  40s                373ms         3s              22s            7s      
#2
Apr 14   No Changes 6s                  40s                373ms         3s              22s            7s
11:11
#1
Apr 14   No Changes 
11:09

Static Code Analysis ( Sonarqube )

Docker 
Install Sonarqube 

How to install on Virtual Machine 
( 3th virtual machine )

> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main   ssh 192.168.1.12
> # ~ clear

Prerequisites
Virtual machine running Ubuntu 22.04 or newer  
Update Packages Repository and Update Packages
sudo apt update
sudo apt upgrade

> sudo apt update
sudo apt upgrade

PostgreSQL
Add PostgresSQL repository
Run from shell prompt 
sudo sh -c ‘ “deb http://apt.postgressql.org/pub/repos/apt $(lsb_release -cs)-pgdg main” > etc/apt/sources.list.d/pgdg.list’
wget -qO- https://www.postgressql.org/media/keys/ACCC4CF8.asc | sudo tee /etc/apt/trusted.gpg.d/pgdg.asc &>/dev/null

Do you want to continue? [Y/n] sudo sh -c ‘ “deb http://apt.postgressql.org/pub/repos/apt $(lsb_release -cs)-pgdg main” > etc/apt/sources.list.d/pgdg.list’
wget -qO- https://www.postgressql.org/media/keys/ACCC4CF8.asc | sudo tee /etc/apt/trusted.gpg.d/pgdg.asc &>/dev/null

> sudo -i
root@sonarqube:~# apt install wget
root@sonarqube:~# clear 
root@sonarqube:~# sudo sh -c ‘ “deb http://apt.postgressql.org/pub/repos/apt $(lsb_release -cs)-pgdg main” > etc/apt/sources.list.d/pgdg.list’
wget -qO- https://www.postgressql.org/media/keys/ACCC4CF8.asc | sudo tee /etc/apt/trusted.gpg.d/pgdg.asc &>/dev/null

root@sonarqube:~#  

Install PostgresSQL 
Run from shell prompt 
sudo apt update 
sudo apt-get -y install postgresql postgresql-contrib
sudo systemctl enable postgresql

root@sonarqub:~#  sudo apt update 
sudo apt-get -y install postgresql postgresql-contrib
sudo systemctl enable postgresql

Create Database for Sonarqube
Set password for postgres user
sudo passwd postgres

root@sonarqub:~# sudo passwd postgres
New password: …
Retype new password: …

Change to the postgres user 
su - postgres

root@sonarqub:~# su - postgres

Set password and grant privileges 
createuser sonar 
psql
ALTER USER sonar WITH ENCRYPTED password ‘sonar’;
CREATE DATABASE sonarqube OWNER sona;
grant all privileges on DATABASE sonarqube to sonar;
\q 
exit 

root@sonarqub:~# createuser sonar
root@sonarqub:~# psql
root@sonarqub:~# ALTER USER sonar WITH ENCRYPTED password ‘sonar’;
CREATE DATABASE sonarqube OWNER sona;
grant all privileges on DATABASE sonarqube to sonar;
\q 

postgres@sonarqube:~# exit 
logout 
postgres@sonarqube:~# clear

Add Adoptium Repository 
wget -0 - https://packages.adoptium.net/artifactory/api/gpg/key/public | tee /etc/apt/keyrings/adoptium.asc
echo “deb [ signed-by=etc/apt/keyrings/adoptium.esc ] https://packages.adoptium.net/artifactory/deb $(awk -F= ‘/^VERSION_CODENAME/{print$2}’ /etc/os-release) main” tee /etc/apt/source.list./adoptium.list

postgres@sonarqube:~# wget -0 - https://packages.adoptium.net/artifactory/api/gpg/key/public | tee /etc/apt/keyrings/adoptium.asc
echo “deb [ signed-by=etc/apt/keyrings/adoptium.esc ] https://packages.adoptium.net/artifactory/deb $(awk -F= ‘/^VERSION_CODENAME/{print$2}’ /etc/os-release) main” tee /etc/apt/source.list./adoptium.list 


Check out our code from our source code private repository.( Need some credentials ) 
Create a username password ( Access Token ) 

Install Java 17 
Update repository and install java 
apt update
apt install temurin-17-jdk
update-alternatives - -config java
/usr/bin/java/ - -version
exite

postgres@sonarqube:~# apt update
apt install temurin-17-jdk

Linux kernel Tuning 
Increase Limits 
Run from prompt shell
sudo vim /etc/security/limits.conf

postgres@sonarqube:~# sudo vim /etc/security/limits.conf
End of file 

Add this values 
sonarqube - no file 65536
sonarqube - nproc 4096

End of file 
sonarqube - no file 65536
sonarqube - nproc 4096
:wq ( Save )

Increase Mapped Memory Regions 
Run from shell prompt 
sudo vim /etc/sysctl.conf

postgres@sonarqube:~# sudo vim /etc/sysctl.conf

Add this files 
vm max_map_count = 262144

vm max_map_count = 262144

:wq ( Save ) 

Reboot System
Run from shell prompt
sudo reboot 

postgres@sonarqube:~# sudo reboot ( Our Virtual Machine ) 

> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main ssh 192.168.1.12

> # ~ clear
> # ~sudo -i 
root@sonarqube:~# 

Sonarqube 
Download and Extract 
Run from shell prompt 
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.0.65466.zip
sudo apt install unzip
sudo unzip sonarqube-9.9.0.65466.zip -d /opt
sudo mv /opt/sonarqube-9.9.0.65466 /opt/sonarqube

root@sonarqube:~# sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.0.65466.zip
sudo apt install unzip
sudo unzip sonarqube-9.9.0.65466.zip -d /opt
sudo mv /opt/sonarqube-9.9.0.65466 /opt/sonarqube

Create user and set permissions 
Run from shell prompt
sudo groupadd sonar
sudo useradd -c “user to run SonarQube” -d /opt/sonarqube -g sonar sonar 
sudo chown sonar:sonar /opt/sonarqube -R

sudo groupadd sonar
sudo useradd -c “user to run SonarQube” -d /opt/sonarqube -g sonar sonar 
sudo chown sonar:sonar /opt/sonarqube -R

root@sonarqube:~# cd /opt/
root@sonarqube:/opt# ls -al

Update Sonarqube properties with DB credentials 
Run from shell prompt 
sudo vim /opt/sonarqube/conf/sonar.properties

root@sonarqube:/opt# sudo vim /opt/sonarqube/conf/sonar.properties

Find and replace the below values, you might need to add the sonar .jdbc.url
Run from shell prompt 
sonar.jdbc.username=sonar
sonar.jdbc.password=sonar
sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube 

Database
…
# User credentials. 
# Permission to create tables, indices and triggers must be granted to JDBC user.
# The schema must be created first.
#sonar.jdbc.username=
#sonar.jdbc.password=
sonar.jdbc.username=sonar
sonar.jdbc.password=sonar
sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube 
:wq

Create service for Sonarqube
Run from shell prompt
sudo vim /etc/systemd/system/sonar.service

root@sonarqube:/opt# sudo vim /etc/systemd/system/sonar.service

Past the below into the file
Past the below contents 
[Unit]
Description=SonarQube service
After=syslog.target network.target 

[Service]
Type=forking

Execstart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
Execstop=/opt/sonarqube.bin/linux-x86-64/sonar.sh stop

User=sonar
Group=sonar
Restart=always 

LimitNOFILE=65536
LimitNPROC=4096

[Install]
WantedBy=multi-user.target
- - - 
[Unit]
Description=SonarQube service
After=syslog.target network.target 

[Service]
Type=forking

Execstart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
Execstop=/opt/sonarqube.bin/linux-x86-64/sonar.sh stop

User=sonar
Group=sonar
Restart=always 

LimitNOFILE=65536
LimitNPROC=4096

[Install]
WantedBy=multi-user.target

Start Sonarqube and Enable service
Past the below contents
sudo systemctl start sonar
sudo systemctl enable sonar
sudo systemctl status sonar

root@sonarqube:/opt# sudo systemctl start sonar
sudo systemctl enable sonar
sudo systemctl status sonar

Watch log files and monitor for startup
Watch logs
sudo tail -r /opt/sonarqube/logs/sonar.log

root@sonarqube:/opt# sudo tail -r /opt/sonarqube/logs/sonar.log

192.168.1.12:9000 ( Virtual Machine ) 

SonarQube is up
All systems operational 
Home 

192.168.1.12:9000

Log in to SonarQube
Admin
…

Update your password 
This account should not use the default password
Enter a new password
Old Password
Admin 
New Password
…
Confirm Password
…
Update

Set up an reverse Proxy ( Access through a Domain Name ) - Secure Url


Jenkins UI ( To SonarQube make an analysis it have to be set up an API )

SonarQube
Search for projects… My account - Security 
Generate Token 

Docker
Optional Reverse Proxy and TLS Configuration 
Run from shell prompt 
sudo apt install nginx 

root@sonarqube:/opt# sudo apt install nginx 

Do you want to continue? [Y/n] Y

create nginx config file
vi /etc/nginx sites-availables/sonarqube.conf

root@sonarqube:/opt# vim vi /etc/nginx sites-availables/sonarqube.conf

Past the content below and make sure to update the domain name
Past and Update
Server {

       listen 80;
       server_name sonarqube.dev.dman.cloud;
       access_log /var/log/nginx/sonar.access_log;
       error_log /var/log/nginx/sonar.error_log;
       proxy_buffers16 64k;
       proxy_buffer_size 128k;

       location / {
              proxy_pass http://127.0.0.1:9000;
              proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504
              proxy_redirect off;
              proxy_set_header Host $host;
              proxy_set_header X-Real_IP $remote addr;
              proxy_set_header X-Forwarded-For $proxy_add_x_forward_for;
              proxy_set_header X-Forwarded-Photo http;
       }
}
- - -
Server {

       listen 80;
       server_name sonarqube.dev.dman.cloud;
       access_log /var/log/nginx/sonar.access_log;
       error_log /var/log/nginx/sonar.error_log;
       proxy_buffers16 64k;
       proxy_buffer_size 128k;

       location / {
              proxy_pass http://127.0.0.1:9000;
              proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504
              proxy_redirect off;
              proxy_set_header Host $host;
              proxy_set_header X-Real_IP $remote addr;
              proxy_set_header X-Forwarded-For $proxy_add_x_forward_for;
              proxy_set_header X-Forwarded-Photo http;
       }
}

:wq

Next, activate the server block configuration ‘sonarqube.conf’ by creating a symlink of that file to the ‘/etc/nginx/sites-enabled’ directory. Then, verify your Nginx configuration files.
Enable virtual host and restart nginx
sudo ln -s /etc/nginx/sites-availables/sonarqube.conf /etc/nginx/sites-enable/
sudo nginx -t
sudo systemctl restart nginx 
root@sonarqube:/opt# sudo ln -s /etc/nginx/sites-availables/sonarqube.conf /etc/nginx/sites-enable/
sudo nginx -t
root@sonarqube:/opt# sudo systemctl restart nginx 

( Not Secure - Install the certificate ) http://sonarqube.dev.dman.cloud 

Log in to SonarQube 
Login
Password 
Login in 

Docker 
Install Certbot
The first step to using Let´s Encrypt to obtain an SSL certificate is to install the certbot software on your server:
sudo apt install certbot phyton3-certbot-nginx 

root@sonarqube:/opt# sudo apt install certbot phyton3-certbot-nginx 
Do you want to continue? [Y/n] y

Obtain an SSL Certificate 
Certbot provides a variety of ways to obtain SSL Certificates through plugins. The Nginx plugin will take care of reconfiguring Nginx and reloading the config whenever necessary. To use this plugin, type the following:
sudo certbot - -nginx -d sonarqube.dev.dman.cloud 

root@sonarqube:/opt# sudo certbot - -nginx -d sonarqube.dev.dman.cloud 

(Enter ‘c’ to cancel) dinesh@dman.clud 
(Y)es/(N)o: Y
(Y)es/(N)o: N

https://sonarqube.dev.dman.cloud

( Running on a secure port ) http://sonarqube.dev.dman.cloud 

Log in to SonarQube 
Login - admin
Password - …
Login in 

Create API Key 
Search for projects… A ( My Account ) 
Security 
Generate Tokens 
Name - jenkins.dev.dman.cloud
Type - Global Analysis Token 
Expires in - No expiration 
Generate 

Copy sqa_7ad1166adb6d95d59e4731c2777acde7edeeb8827

Jenkins UI 
Dashboard
Manage Jenkins 
Manage Credentials ( Configure credentials ) 
Stores Scoped to Jenkins
                                        Domains (Global) 
                                        Add credentials 
New credentials 
Kind 
Secret text
Secret - sqa_7ad1166adb6d95d59e4731c2777acde7edeeb8827
ID - jenkins-sonarqube-token ( To reference the pipeline ) 
Description ? - jenkins-sonarqube-token
Create 

Manage Jenkins 
Manage Plugins ( Add, remove,disable or enable plugins that can extend the functionality of Jenkins )
Available plugins 
Plugins ( sonarqube ) - search 
SonarQube Scanner 
Plugins ( quality ) - search 
Sonar Quality Gates 
Quality Gates 
Download now and install after restart 
Download progress - Restart Jenkins when installation is complete and no jobs are running

Visual Studio Code
Jenkinsfile - https://github.com/dmancloud/complete-prodcution-e2e-pipeline
pipeline{
              agent 
                     label “jenkins-agent”
              tools{
                     jdk ‘java17’
                     maven ‘Maven3’
              }
              stages{ 
                     stage(“Cleanup Workspace”){
                            steps{  
                                   cleanWs{} ( Build Function that comes with Jenkins ) 
                            }
                     }
      
                     stage(“Checkout from SCM”){
                            steps{  
                                 git branch: ‘main’, credentialsId: ‘github’, url: ‘https://github.com/dmancloud/complete-prodcution-e2e-pipeline
’
                            }
                     }

                     stage(“Build Application”){
                            steps{  
                                   sh “mvn clean package”   
                            }
                     }

                    stage(“Test Application”){
                            steps{  
                                   sh “mvn test ”    
                            }
                     }

                      stage(“Sonarqube Analysis”){
                            steps{  
                                   withSonarQubeEnv(credentialsId: ‘jenkins-sonarqube-token’) {
                                         sh “mvn sonar:sonar”    
                                   }
                            }
                     }
               }
       }

Jenkins UI

jenkins.dev.dman.cloud

Welcome to Jenkins
admin 
…
Keep me signed in 
Sign in 

Dashboard
Manage Jenkins
Configure System ( Configure global settings and paths. )
Configure System
SonarQube server - ( Environment variables ) Add SonarQube
Name ( add a scanner ) 
sonarqube-server
Server URL
Default is http://localhost:9000
https://sonarqube.dev.dman.cloud
Server authentication token 
SonarQube authentication token. Mandatory when anonymous access is disabled
jenkins-sonarqube-token
Apply
Save

Dashboard
Manage Jenkins
Global Tool Configuration (Configure tools, their locations and automatic installers.) 
Global Tool Configuration 
SonarQube Scanner - Add SonarQube Scanner
Name- sonarqube-scanner-latest 
Apply
Save

Manage Jenkins 
Warning … 
Configure which of these warnings are shown 
Configure Global Security 
Hidden security warnings 
Security warnings - Disable both 
Apply 
Save

Shell Terminal 2
> # ~ 
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !1 clear
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !3 ?1 git add.
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !3 ?1 git commit -m “added sonarqube scanner”
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !3 ?1 git push origin main 
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main 

Jenkins UI 
Dashboard
Name - Run the Pipeline 

complete-production-e2e-pipeline
Pipeline Syntax
Open Link in New Tab
Overview
Steps 
Sample Step - withSonarQubeEnv: Prepare SonarQube Scanner environment 
Server authentications token ( jenkins-sonarqube-token ) Can not find any credentials with id jenkins-sonarqube-token
Generate Pipeline Scrypt 
withSonarQubeEnv(credentialsId: ‘jenkins-sonarqube-token’)
Jenkins UI 
Dashboard
Manage Credentials 
Manage Credentials (Configure credentials)
Credentials 
Secret text 

Jenkins UI 
Dashboard
Manage Credentials 
Secret text - jenkins-sonarqube-token ( Delete ) 

Stores Scoped to Jenkins - Domain (global) Add credentials
New credentials 
Kind - Secret text 

Sonarqube - A Administrator 

Secret -  sqa_7ad1166adb6d95d59e4731c2777acde7edeeb8827
ID - (jenkins-sonarqube-token)
Description - (jenkins-sonarqube-token)
Create
Server authentications token ( jenkins-sonarqube-token )  Can not find any credentials with id jenkins-sonarqube-token

Jenkins UI
Dashboard
complete-production-e2e-pipeline
Manage Jenkins 
Configure System (Configure global settings and paths.)
Configure System
Environment variables - Enable 
SonarQube servers
Server authentication token - (jenkins-sonarqube-token)
Name - sonarqube-server
Apply 
Save

Manage Jenkins 
Global Tool Configuration ( Configure tools, their locations and automatic installers ) 
SonarQube Scanner
Name
sonarqube-scanner 
Apply 
Save 

Shell Terminal 2
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main git status
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !1 git add .
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !1 git commit -m “add sonarqube scanner”
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !1 git push origin main 
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main 

Jenkins UI 
Dashboard
complete-production-e2e-pipeline
Build Now
#6
Console Output

Visual Studio Code
Jenkinsfile - https://github.com/dmancloud/complete-prodcution-e2e-pipeline
pipeline{
              agent 
                     label “jenkins-agent”
              tools{
                     jdk ‘java17’
                     maven ‘Maven3’
              }
              stages{ 
                     stage(“Cleanup Workspace”){
                            steps{  
                                   cleanWs{} ( Build Function that comes with Jenkins ) 
                            }
                     }
      
                     stage(“Checkout from SCM”){
                            steps{  
                                 git branch: ‘main’, credentialsId: ‘github’, url: ‘https://github.com/dmancloud/complete-prodcution-e2e-pipeline
’
                            }
                     }

                     stage(“Build Application”){
                            steps{  
                                   sh “mvn clean package”   
                            }
                     }

                    stage(“Test Application”){
                            steps{  
                                   sh “mvn test ”    
                            }
                     }

                      stage(“Sonarqube Analysis”){
                            steps{  
                                   script {
                                          withSonarQubeEnv(credentialsId: ‘jenkins-sonarqube-token’) {
                                                 sh “mvn sonar:sonar”    
                                          }
                                   } 
                            }
                     }
               }
       }

Shell Terminal 2
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main git add .
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !1 git commit -m “added sonarqube scanner”
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !1 git push origin main 
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main 

Jenkins 
Dashboar
complete-production-e2e-pipeline
Build Now
#7

Pipeline complete-production-e2e-pipeline
Stage View 


                                   Declarative   Declarative:   Cleanup       Checkout   Build           Test 
                                   Checkout      Tool Install     Workspace  from SCM  Application                                                  SCM                           SCM

Application Analysis      
                                   3s                 140ms            305ms        3s               15s          7s
2s

#7                              
April 14 tree commits 3s                 183ms            355ms         2s
11:50                 
#3 
Apr 14
11:14 No Changes     1s 
Average stage times  6s                  40s                373ms         3s              22s            7s      
#2
Apr 14   No Changes 6s                  40s                373ms         3s              22s            7s
11:11
#1
Apr 14   No Changes 
11:09

sonarqube.dev.dman.cloud/projects ( Sonarqube )
demoapp ( passed ) 

Bugs   Vulnerabilities   Hotspots Reviewed   Code Smells   Coverage   Duplications   Lines
A         A                       A                               A                     0.0%           0.0%              102 XS XML Java

Quality Gate 
sonarqube.dev.dman.cloud/projects ( If it does not happens ) 
Block the Deployment - To do that
Webhook that respond back to Jenkins 
Sonarqube - Administration 
Configuration
Webhooks
Create
Create Webhook
Name - ( jenkins-dev.dman.cloud )
URL - https://jenkins.dev.dman.cloud/sonarqube-webhook/
Create 

Visual Studio Code
Jenkinsfile - https://github.com/dmancloud/complete-prodcution-e2e-pipeline
pipeline{
              agent 
                     label “jenkins-agent”
              tools{
                     jdk ‘java17’
                     maven ‘Maven3’
              }
              stages{ 
                     stage(“Cleanup Workspace”){
                            steps{  
                                   cleanWs{} ( Build Function that comes with Jenkins ) 
                            }
                     }
      
                     stage(“Checkout from SCM”){
                            steps{  
                                 git branch: ‘main’, credentialsId: ‘github’, url: ‘https://github.com/dmancloud/complete-prodcution-e2e-pipeline
’
                            }
                     }

                     stage(“Build Application”){
                            steps{  
                                   sh “mvn clean package”   
                            }
                     }

                    stage(“Test Application”){
                            steps{  
                                   sh “mvn test ”    
                            }
                     }

                      stage(“Sonarqube Analysis”){
                            steps{  
                                   script {
                                          withSonarQubeEnv(credentialsId: ‘jenkins-sonarqube-token’) {
                                                 sh “mvn sonar:sonar”    
                                          }
                                   } 
                            }
                     }
               
                     stage(“Quality Gate”){
                            steps{  
                                   script {
                                          waitForQualityGate abortPipeline: false, credentialsId: ‘jenkins-sonarqube-token’                                          
                                   } 
                            }
                     }
               }
       }

Shell Terminal 2
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main git add .
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main +1 git commit -m “added quality gate”
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main +1 git push origin main 
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main 

Jenkins UI
complete-production-e2e-pipeline 
Build Now

                                   Declarative   Declarative:   Cleanup       Checkout   Build           Test 
                                   Checkout      Tool Install     Workspace  from SCM  Application                                                  SCM                           SCM

Application   Quality 
Analysis       Gate    
2s                               123ms           273ms           3s               13s             7s            14s
                                   3s                 140ms            305ms        3s               15s          7s
#8                155ms

#7                              
April 14 tree commits 3s                 183ms            355ms         2s
11:50                 
#3 
Apr 14
11:14 No Changes     1s 
Average stage times  6s                  40s                373ms         3s              22s            7s      
#2
Apr 14   No Changes 6s                  40s                373ms         3s              22s            7s
11:11
#1
Apr 14   No Changes 
11:09

Refresh sonarqube page 

Build and Push Docker Container 
Install plugins 
Jenkins
Dashboard
Manage Jenkins 
Manage Plugins (Add, remove, disable or enable plugins that can extend the functionality of Jenkins )
Available Plugins
Plugins - Search
Docker 
Docker Commons 
Docker Pipeline
Docker API
docker-build-step
CloudBees Docker Build and Publish
Download now and install after restart
Restart jenkins when installation is complete and no jobs are running

Environment Variables

Visual Studio Code
Jenkinsfile - https://github.com/dmancloud/complete-prodcution-e2e-pipeline
pipeline{
              agent 
                     label “jenkins-agent”
              tools{
                     jdk ‘java17’
                     maven ‘Maven3’
              }
              environment {
                     APP_NAME = “complete-production-e2e-pipeline”
                     RELEASE = “1.0.0”
                     DOCKER_USER = “dmancloud”
                     DOCKER_PASS = ‘dockerhub’
                     IMAGE_NAME = “${DOCKER_USER}” + “/” + “${APP_NAME}
                     IMAGE_TAG = “${RELEASE}-$(BUILD_NUMBER)”
              }
              stages{ 
                     stage(“Cleanup Workspace”){
                            steps{  
                                   cleanWs{} ( Build Function that comes with Jenkins ) 
                            }
                     }
      
                     stage(“Checkout from SCM”){
                            steps{  
                                 git branch: ‘main’, credentialsId: ‘github’, url: ‘https://github.com/dmancloud/complete-prodcution-e2e-pipeline
’
                            }
                     }

                     stage(“Build Application”){
                            steps{  
                                   sh “mvn clean package”   
                            }
                     }

                    stage(“Test Application”){
                            steps{  
                                   sh “mvn test ”    
                            }
                     }

                      stage(“Sonarqube Analysis”){
                            steps{  
                                   script {
                                          withSonarQubeEnv(credentialsId: ‘jenkins-sonarqube-token’) {
                                                 sh “mvn sonar:sonar”    
                                          }
                                   } 
                            }
                     }
               
                     stage(“Quality Gate”){
                            steps{  
                                   script {
                                          waitForQualityGate abortPipeline: false, credentialsId: ‘jenkins-sonarqube-token’                                          
                                   } 
                            }
                     }
               }
       }

Jenkins UI 
Dashboard
Manage Jenkins 
Manage Plugins (Add, remove, disable or enable plugins that can extend the functionality of Jenkins )
Stores Scoped to Jenkins
Domains ( Global ) - Add credentials 
New Credentials 
Kind
Username ? ( dmancloud ) - Dockerhub sign 
Dockerhub Access Token - dckr_pat_iYBTthNa-re36QxNmdHU5vkCSY
Password: dckr_pat_iYBTthNa-re36QxNmdHU5vkCSY
ID - dockerhub
Description ? - dockerhub
Create 

Visual Studio Code
Jenkinsfile - https://github.com/dmancloud/complete-prodcution-e2e-pipeline
pipeline{
              agent 
                     label “jenkins-agent”
              tools{
                     jdk ‘java17’
                     maven ‘Maven3’
              }
              environment {
                     APP_NAME = “complete-production-e2e-pipeline”
                     RELEASE = “1.0.0”
                     DOCKER_USER = “dmancloud”
                     DOCKER_PASS = ‘dockerhub’
                     IMAGE_NAME = “${DOCKER_USER}” + “/” + “${APP_NAME}
                     IMAGE_TAG = “${RELEASE}-$(BUILD_NUMBER)”
              }
              stages{ 
                     stage(“Cleanup Workspace”){
                            steps{  
                                   cleanWs{} ( Build Function that comes with Jenkins ) 
                            }
                     }
      
                     stage(“Checkout from SCM”){
                            steps{  
                                 git branch: ‘main’, credentialsId: ‘github’, url: ‘https://github.com/dmancloud/complete-prodcution-e2e-pipeline
’
                            }
                     }

                     stage(“Build Application”){
                            steps{  
                                   sh “mvn clean package”   
                            }
                     }

                    stage(“Test Application”){
                            steps{  
                                   sh “mvn test ”    
                            }
                     }

                      stage(“Sonarqube Analysis”){
                            steps{  
                                   script {
                                          withSonarQubeEnv(credentialsId: ‘jenkins-sonarqube-token’) {
                                                 sh “mvn sonar:sonar”    
                                          }
                                   } 
                            }
                     }
               
                     stage(“Quality Gate”){
                            steps{  
                                   script {
                                          waitForQualityGate abortPipeline: false, credentialsId: ‘jenkins-sonarqube-token’                                          
                                   } 
                            }
                     }

                     stage(“Build & Push Docker Image”){
                            steps{  
                                   script {
                                          docker.withRegistry (‘’, Docker_PASS) {
                                                docker_image = docker.build “${IMAGE_NAME}”
                                          }          
                                         
                                          docker.withRegistry(‘’, Docker_PASS) {
                                                        docker_image.push(“${IMAGE_TAG}”)
                                                        docker_image.push(‘latest’)
                                                 }      
                                          }                          
                                   } 
                            }
                     }
               }
       }

Shell Terminal 2
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main git add.
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main +1 git commit -m “added docker build and push”
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main +1 git push origin main
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main 

Jenkins UI
Dashboard 
Name
complete-production-e2e-pipeline
Pipeline complete-production-e2e-pipeline
Build Now 

                                   Declarative   Declarative:   Cleanup       Checkout   Build           Test 
                                   Checkout      Tool Install     Workspace  from SCM  Application                                                  SCM                           SCM

Application   Quality 
Analysis       Gate    
2s                               123ms           273ms           3s               13s             7s            14s
                                   3s                 140ms            305ms        3s               15s          7s


#9 

#8                155ms

#7                              
April 14 tree commits 3s                 183ms            355ms         2s
11:50                 
#3 
Apr 14
11:14 No Changes     1s 
Average stage times  6s                  40s                373ms         3s              22s            7s      
#2
Apr 14   No Changes 6s                  40s                373ms         3s              22s            7s
11:11
#1
Apr 14   No Changes 
11:09

#9 - Console Output 

Multi Stage Build Process 
Visual Studio Code 
Dockerfile

From maven:3.9.0-eclipse-temurin 17 as build
WORKDIR /app
COPY
RUN mvn clean install

FROM eclipse-temurin:17.0.6_10-jdk
WORKDIR /app
COPY - - from=build /app/target/demoapp.jar /apps/
EXPOSE  8080
CMD [“java”, “-jar”, “demoapp.jar”]

Shell Terminal 2
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main git add .
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main +1 git commit -m “added dockerfile”
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main +1 git push origin main 
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main 

Jenkins UI
Dashboard 
Name
complete-production-e2e-pipeline
Pipeline complete-production-e2e-pipeline
Build Now 

                                   Declarative   Declarative:   Cleanup       Checkout   Build           Test 
                                   Checkout      Tool Install     Workspace  from SCM  Application                                                  SCM                           SCM

Application   Quality   Build and Push 
Analysis       Gate       Docker Image 

2s                               123ms           273ms           3s               13s             7s            14s
                                   3s                 140ms            305ms        3s               15s          7s

#10                             765ms           117ms           219ms         4s               8s            7s
10s               272ms    54s
       
#9 

#8                155ms

#7                              
April 14 tree commits 3s                 183ms            355ms         2s
11:50                 
#3 
Apr 14
11:14 No Changes     1s 
Average stage times  6s                  40s                373ms         3s              22s            7s      
#2
Apr 14   No Changes 6s                  40s                373ms         3s              22s            7s
11:11
#1
Apr 14   No Changes 
11:09

hub.docker.com ( Refresh Page )

dmancloud / complete-production-e2e-pipeline 
Contains: Image | Last published a few seconds ago
Tags 
Tag
                Type
latest        Image
1.0.0-10   Image

Deploy Kubernetes Cluster 

Two Virtual Machines ( Set up ) ArgoCD and Kubernetes 
Docker
ArgoCD
Install ArgoCD 


Update Package Repository and Upgrade Packages 
Run from shell prompt
sudo apt update
sudo apt upgrade

Shell Terminal 2 
> ssh 192.168.1.13
> # ~ sudo apt update
sudo apt upgrade


Shell Terminal 3 
> ssh 192.168.1.20
> # ~ sudo apt update
sudo apt upgrade

Create Kubernetes Cluster 
Run from shell prompt
sudo bash
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC= “server” sh -s -  --disable traefik
exit
mkdir .kube
sudo cp /etc/rancher/k3s/k3s.yaml ./config
sudo chown dmistry:dmistry config
chmod 400 config
export KUBECONFIG=~/. kube config

Shell Terminal 2 
> ssh 192.168.1.13
> # ~ sudo apt update
sudo apt upgrade

Do you want to continue? [Y/n] y
> # ~ curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC= “server” sh -s -  --disable traefik
root@argocd-cluster:home/dmistry# sudo bash
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC= “server” sh -s -  --disable traefik
root@argocd-cluster:home/dmistry# clear

Shell Terminal 3 
> ssh 192.168.1.20
> # ~ sudo apt update
sudo apt upgrade

> # ~ sudo bash
root@argocd-cluster:home/dmistry# clear
root@argocd-cluster:home/dmistry# curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC= “server” sh -s -  --disable traefik
root@argocd-cluster:home/dmistry# clear

Shell Terminal 2 
root@argocd-cluster:home/dmistry# exit
> # ~

Shell Terminal 3 
> # ~ clear 

Shell Terminal 2 
> # ~ clear 
> # ~ mkdir .kube

Shell Terminal 3 
> # ~ mkdir .kube

Shell Terminal 2 
> # ~ cd .kube/
> # ~/. kube


Shell Terminal 3 
> # ~ cd .kube/
> # ~/. kube

Shell Terminal 2 
> # ~/. kube sudo cp /etc/rancher/k3s/k3s.yaml ./config
sudo chown dmistry:dmistry config
chmod 400 config
export KUBECONFIG=~/. kube config
> # ~/. kube 

Shell Terminal 3 
> # ~/. kube sudo cp /etc/rancher/k3s/k3s.yaml ./config
sudo chown dmistry:dmistry config
chmod 400 config
export KUBECONFIG=~/. kube config
> # ~/. kube 

Shell Terminal 2 
> # ~/. kube kubectl get nodes 
NAME               STATUS   ROLES                       AGE   VERSION
argocd-cluster  Ready       control-plane,master  65s     v1.263+k3s1
> # ~/. kube 

Shell Terminal 3 
> # ~/. kube kubectl get nodes 
NAME               STATUS   ROLES                       AGE   VERSION
argocd-cluster  Ready       control-plane,master  65s     v1.263+k3s1
> # ~/. kube 

Install ArgoCD
Run from shell prompt
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubsercontent.com/argoproj/argo-cd/stable/manifest/install.yaml

Shell Terminal 2
> # ~/. kube kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubsercontent.com/argoproj/argo-cd/stable/manifest/install.yaml

Change Service to NodePort 
Edit the service can change the service type from ClusterIp to NodePort
Run from prompt shell
kubectl patch svc argo-server -n argocd -p ‘{“spec”: {“type”: “NodePort”}}’

Shell Terminal 2
> # ~/. kube kubectl patch svc argo-server -n argocd -p ‘{“spec”: {“type”: “NodePort”}}’
> # ~/. kube 

Fetch password 
Run from shell prompt
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath=”{.data.password}” | base64 -d

Shell Terminal 2
> # ~/. kube kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath=”{.data.password}” | base64 -d
fw3kjV-LeKORwyv9
> # ~/. kube 

Shell Terminal 3 
> # ~/. kube fw3kjV-LeKORwyv9

Shell Terminal 2
Optional (Enable TLS w/Ingress)
If you want to enable access from the internet or private network you can follow the instructions below to install and configure an ingress-controller with lets-encrypt. 
Install Cert Management 
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install \
   cert-manager jetstack/cert-manager \
   - - namespace cert-manager \ 
   - - create-namespace \
   - - version v1.11.0 \
   - - set installCRDs=true

> # ~/. kube helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install \
   cert-manager jetstack/cert-manager \
   - - namespace cert-manager \ 
   - - create-namespace \
   - - version v1.11.0 \
   - - set installCRDs=true

Create cluster Issuser for Lets Encrypt vi letsencrypt-product.yalm and past below contents adjust your email address.
apiVersion: cert-manager.io/v1
kind: ClusterIssuer 
metadata
   name: letsencrypt-prod
spec: 
   ecme:
      server: https://acme-v2.api.letsencrypt.org/directory
      email: dinesh@dmancloud
      privateKeySecretRef:
         name: letsencrypt-prod
      solvers:
         - http01:
              ingress:
                 class: nginx 

Shell Terminal 2
> # ~/. kube vi letsencrypt-product.yalm 

> # ~/. kube apiVersion: cert-manager.io/v1
kind: ClusterIssuer 
metadata
   name: letsencrypt-prod
spec: 
   ecme:
      server: https://acme-v2.api.letsencrypt.org/directory
      email: dinesh@dmancloud
      privateKeySecretRef:
         name: letsencrypt-prod
      solvers:
         - http01:
              ingress:
                 class: nginx 

Apply manifest
kubectl apply -f letsencrypt-product.yaml

> # ~/. kube kubectl apply -f letsencrypt-product.yaml
> # ~/. kube kubectl get ClusterIssuer -A
NAME                   READY   ME
letsencrypt-prod    true         13s
> # ~/. kube 

Deploy nginx-ingress controller 
Apply manifest
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.7.0/deploy/static/provider/cloud/deploy.yaml

> # ~/. kube kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.7.0/deploy/static/provider/cloud/deploy.yaml
> # ~/. kube clear

> # ~/. kube get svc -A 
> # ~/. kube clear

> # ~/. kube 

Create ingress for ArgoCD vi ingress.yaml and past the below contents adjust the domain name.
Apply manifest
apiVersion: networking.k8s.io/v1
Kind: Ingress
metadata
   name: argocd-server-ingress
   namespace: argocd
   annotations:
      cert-manager.io/cluster-issuer/ letsencrypt-prod
      kubernetes.io/ingress.class: nginx 
      kubernetes.io/tls-acme: “true”
      # if you encounter a redirect loop or are getting a 307 response code
      # then you need to force the nginx ingress to connect to the backend using HTTPS.
      # 
      nginx.ingress.kubernetes.io/backend-protocol: “HTTPS”
spec:
   rules:
host: argocd-dev.dman.cloud
http:
 - paths: /
   pathType: Prefix
   backend:
      service:
         name: argocd-server
         port:
            name: https
tls
   - hosts:
     - argocd.dev.dman.cloud
     secretName: argo-secret # do not change, this is provided by Argo CD

> # ~/. kube vim ingress.yaml

- - -
apiVersion: networking.k8s.io/v1
Kind: Ingress
metadata
   name: argocd-server-ingress
   namespace: argocd
   annotations:
      cert-manager.io/cluster-issuer/ letsencrypt-prod
      kubernetes.io/ingress.class: nginx 
      kubernetes.io/tls-acme: “true”
      # if you encounter a redirect loop or are getting a 307 response code
      # then you need to force the nginx ingress to connect to the backend using HTTPS.
      # 
      nginx.ingress.kubernetes.io/backend-protocol: “HTTPS”
spec:
   rules:
host: argocd-dev.dman.cloud
http:
 - paths: /
   pathType: Prefix
   backend:
      service:
         name: argocd-server
         port:
            name: https
tls
   - hosts:
     - argocd.dev.dman.cloud
     secretName: argo-secret # do not change, this is provided by Argo CD

Apply manifest 
kubectl apply -f ingress.yaml

> # ~/. kube kubectl apply -f ingress.yaml
> # ~/. kube get ingress -A
NAMESPACE  NAME                           CLASS   HOSTS                           ADRESS  PORT   AGE                                                                                                                             80, 443
35s                                                                                                                              

argocd             argocd-server-ingress   <none>   argocd.dev.dman.cloud

Docker hub

https://argocd.dev.dman.cloud

Let´s staff get deployed!
Argo 
Username - admin
Passaword - fw3kjV-LeKORwyv9
SIGN IN 

Argo CD 

Shell Terminal 3 ( Application Cluster ) 
> # ~ clear
> # ~/. kube cat config
Copy 
ApiVersion: v1 
cluster:
…

Shell Terminal 2 
> # ~/. kube vi app-cluster.yaml
Past
ApiVersion: v1 
cluster:
…
server: https://127.0.0.1:6443 ( Change for 129.168.1.20:6443 ) 
…

> # ~/. kube export KUBECONFIG=~/.kube/app-cluster.yaml
> # ~/. kube clear

> # ~/. kube kubectl get nodes
NAME          STATUS  ROLES                        AGE     VERSION
app-cluster   Ready     control-plane,master   8m21s  v1.26..3+k3s1

> # ~/. kube

Docker ( Install ArgoCD ) 
Install ArgoCD command line tool 
Download With Curl
Run from shell 
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/release/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64

> # ~/. kube curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/release/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64

https://itnext.io/argocd-setup-external-clusters-by-name-d3d58a53acb0
argocd cluster add kind-dev-cluster - - name dev-cluster 

> # ~/. kube argocd cluster add kind-dev-cluster - - name app-cluster

Shell Terminal 3 
> # ~/. kube k get nodes
NAME         STATUS ROLES                       AGE VERSION 
app-cluster  Ready    control-plane,master  11m   vi.26.3+k3s1

Shell Terminal 2 
> # ~/. kube argocd login 
> # ~/. kube export KUBECONFIG=~/.kube/config
> # ~/. kube kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath= “{.data.password}” | base64 -d

fw3kjV-LeKORwyv9

> # ~/. kube kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath= “{.data.password}” | base64 -d

> # ~/. kube export KUBECONFIG=~/.kube/app-cluster.yaml
> # ~/. kube k get nodes
NAME         STATUS ROLES                       AGE VERSION 
app-cluster  Ready    control-plane,master  11m   vi.26.3+k3s1
> # ~/. kube argocd login argocd.dev.dman.cloud
Username:admin
Password: … (fw3kjV-LeKORwyv9)
‘admin login’ login in successfully 
Context ‘argocd.dev.dman.cloud’ updated

> # ~/. kube argoced cluster add default - -name app-cluster
Do you want to continue? [y/N] y

> # ~/. kube 

argocd.dev.dman.cloud/settings/clusters
Argo CD
NAME           URL VERSION                   CONNECTION  STATUS
app-cluster    https://192.168.1.20:6443                             Unknown 

Set up a second repository 
Create a deployment 
Service Manifest 
https://github.com/dmancloud/gitops-complete-prodcution-e2e-pipeline

> # ~/. kube clear 
> # ~/. kube cd
> # ~ 
Connection to 192.168.1.13 closed.
> # ~ ls 
Applications   Documents   Library   Music      Projects 
Desktop          Downloads   Movies   Pictures  Public 
> # ~ cd Projects/dmancloud 
> # ~/Pr/dmancloud ls
complete-productions-e2e-pipeline    gitops-complete-production-e2e-pipeline
> # ~/Pr/dmancloud cd gitops-complete-prodcution-e2e-pipeline
> # ~/Pr/dmancloud ls
README.md deplyment.yaml service.yaml

> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main vim deployment.yaml
apiVersion: apps/v1
kind: deployment
metadata:
   name: complete-production-e2e-deployment
spec:
   replicas: 2
   selector:
      matchLabels:
      app: complete-production-e2e-app
   templates:
      metadata:
         labels:
            app: complete-production-e2e-app
      spec: 
         containers:
             - name: complete-production-e2e-app
               image: dmancloud/complete-production-e2e-pipeline: CHANGEME               resources: 
                  limits:  
                     memory: “256Mi”
                     cpu: “500m”
                ports:  
containerPort: 8080

hub.docker.com
dmancloud
dmancloud / complete-production-e2e-pipeline
Contains Image / Last publish 23 ago
Docker commands
To push a new tag to this repository.
Docker push dmancloud/complete-production-e2e-pipeline 

Tags
This repository contains 2 tag(s).
Tag
1.0.0-10

apiVersion: apps/v1
kind: deployment
metadata:
   name: complete-production-e2e-deployment
spec:
   replicas: 2
   selector:
      matchLabels:
      app: complete-production-e2e-app
   templates:
      metadata:
         labels:
            app: complete-production-e2e-app
      spec: 
         containers:
             - name: complete-production-e2e-app
               image: dmancloud/complete-production-e2e-pipeline: 1.0.0-10
               resources: 
                  limits:  
                     memory: “256Mi”
                     cpu: “500m”
                ports:  
containerPort: 8080

Shell Terminal 2
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main !1 git add .
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main !1 git commit -m “update image tag”
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main !1 git push origin main 
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main 

https://github.com/dmancloud/gitops-complete-prodcution-e2e-pipeline ( Refresh Page - image: dmancloud/complete-production-e2e-pipeline: 1.0.0-10 ) 

To create an application and specify the credentials on github repository inside on Argo CD UI.

Argo CD
Settings 
Repositories 
CONNECT REPO 
Choose your connection method:
Via HTTPS

CONNECT REPO USING HTTPS 
Project - default 
Repository URL - https://github.com/dmancloud/gitops-complete-prodcution-e2e-pipeline
Username (Optional) - dmancloud
Password (Optional) - Github Personal Access Token ( ghp_tt5kWEH6ZulqzO7GuUNQQNWEHQFFI63pj0OT )
CONNECT 

Argo CD 
TYPE           NAME            REPOSITORY                     CONNECTION    STATUS
git                                       https://github.com/dmancloud/gitops-complete-prodcution-e2e-pipeline    successful 

Argo CD
Applications 

No applications available to you just yet
Create new application to start managing resources in your cluster 
CREATE APPLICATION 

> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main cat deployment.yaml

apiVersion: apps/v1
kind: deployment
metadata:
   name: complete-production-e2e-deployment
spec:
   replicas: 2
   selector:
      matchLabels:
      app: complete-production-e2e-app
   templates:
      metadata:
         labels:
            app: complete-production-e2e-app
      spec: 
         containers:
             - name: complete-production-e2e-app
               image: dmancloud/complete-production-e2e-pipeline: 1.0.0-10
               resources: 
                  limits:  
                     memory: “256Mi”
                     cpu: “500m”
                ports:  
containerPort: 8080

> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main

GENERAL
Application Name - complete-production-e2e-pipeline
Project Name - default
SYNC POLICE - Automatic
PRUNE RESOUCERS
SELF HEAL
Source 
Repository URL - https://github.com/dmancloud/gitops-complete-prodcution-e2e-pipeline
Revision - HEAD
Path - ./
DESTINATION
Cluster URL - https://192.168.1.20:6443
Namespace - default
CREATE

Shell Terminal 3
> # ~/. kube clear
> # ~/. kube kubectl get pods -A
NAMESPACE NAME                                                                                   READY STATUS REST ARTS AGE  
default             complete-prodcution-e2e-deployment-59f8c44c9b-x78b6   0/1        CrashLoopBackOff
…

Multi Stage Build Process 
Visual Studio Code 
Dockerfile

From maven:3.9.0-eclipse-temurin 17 as build
WORKDIR /app
COPY
RUN mvn clean install

FROM eclipse-temurin:17.0.6_10-jdk
WORKDIR /app
COPY - - from=build /app/target/demoapp.jar /apps/
EXPOSE  8080
CMD [“java”, “-jar”, “demoapp.jar”]

> # ~/. kube kubectl describe pod  complete-prodcution-e2e-deployment-59f8c44c9b-x78b6

Shell Terminal 2
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main ls
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main cd ../
> # ~/Pr/dmancloud cd omplete-production-e2e-pipeline
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main cd target/
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main ls
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main 

Multi Stage Build Process 
Visual Studio Code 
Dockerfile

From maven:3.9.0-eclipse-temurin 17 as build
WORKDIR /app
COPY
RUN mvn clean install

FROM eclipse-temurin:17.0.6_10-jdk
WORKDIR /app
COPY - - from=build /app/target/demoapp.jar /app/
EXPOSE  8080
CMD [“java”, “-jar”, “demoapp.jar”]

Shell Terminal 2
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main cd ../
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main ls
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main !1 git status 
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main !1 git add .
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main +1  git commit -m “fixed type in dockerfile”
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main +1 git push origin main 
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main 

Jenkins UI
Pipeline complete-production-e2e-pipeline
Stage View
Build Now


                                   Declarative   Declarative:   Cleanup       Checkout   Build           Test 
                                   Checkout      Tool Install     Workspace  from SCM  Application                                                  SCM                           SCM

Application   Quality   Build and Push 
Analysis       Gate       Docker Image 

2s                               123ms           273ms           3s               13s             7s            14s
                                   3s                 140ms            305ms        3s               15s          7s

#11

#10                             765ms           117ms           219ms         4s               8s            7s
10s               272ms    54s
       
#9 

#8                155ms

#7                              
April 14 tree commits 3s                 183ms            355ms         2s
11:50                 
#3 
Apr 14
11:14 No Changes     1s 
Average stage times  6s                  40s                373ms         3s              22s            7s      
#2
Apr 14   No Changes 6s                  40s                373ms         3s              22s            7s
11:11
#1
Apr 14   No Changes 
11:09

Shell Terminal 2 
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main cd ../
> # ~/Pr/dmancloud cd ../
> # ~/Pr/dmancloud cd gitops-complete-production-e2e-pipeline

> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main vim deployment.yaml

apiVersion: apps/v1
kind: deployment
metadata:
   name: complete-production-e2e-deployment
spec:
   replicas: 2
   selector:
      matchLabels:
      app: complete-production-e2e-app
   templates:
      metadata:
         labels:
            app: complete-production-e2e-app
      spec: 
         containers:
             - name: complete-production-e2e-app
               image: dmancloud/complete-production-e2e-pipeline: 1.0.0-10
               resources: 
                  limits:  
                     memory: “256Mi”
                     cpu: “500m”
                ports:  
containerPort: 8080
- - -

apiVersion: apps/v1
kind: deployment
metadata:
   name: complete-production-e2e-deployment
spec:
   replicas: 2
   selector:
      matchLabels:
      app: complete-production-e2e-app
   templates:
      metadata:
         labels:
            app: complete-production-e2e-app
      spec: 
         containers:
             - name: complete-production-e2e-app
               image: dmancloud/complete-production-e2e-pipeline: 1.0.0-11
               resources: 
                  limits:  
                     memory: “256Mi”
                     cpu: “500m”
                ports:  
containerPort: 8080

Shell Terminal 2
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main !1 git add .
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main +1 git commit -m “update image tag”


hub.docker.com/repository/docker/dmancloud/complete-production-e2e-pipeline/general ( Refresh Page ) 
General 
dmancloud / complete-production-e2e-pipeline 
Tags
This repository contain 3 tag(s)  Type
latest                                           image
1.0.0-11                                       image
1.0.0-10                                       image

> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main +1 push origin main
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main 

Argo CD UI
Applications complete-production-e2e-pipeline
SYNC - SYNCHRONIZE 

Shell Terminal 3 
> # ~/.kube clear
> # ~/.kube kubectl get pods -A
NAMESPACE NAME                                                                                   READY STATUS REST ARTS AGE  
default             complete-prodcution-e2e-deployment-59f8c44c9b-x78b6   0/1        CrashLoopBackOff
default             complete-production-e2e-deployment-84cdbd9fb7-6nzvm               running
> # ~/.kube

Argo CD UI 
Applications
complete-production-e2e-pipeline
SYNC - SYNCHRONIZE
SYNC APPS - SYNC
APPS(ALL/OUT OF SYNC/NONE) - COMPLETE_PRODUCTION_E2E_PIPELINE

Shell Terminal 3 
> # ~/.kube clear
> # ~/.kube kubectl get pods -A
NAMESPACE NAME                                                                                   READY STATUS REST ARTS AGE  
default             complete-prodcution-e2e-deployment-59f8c44c9b-x78b6   0/1        CrashLoopBackOff
default             complete-production-e2e-deployment-84cdbd9fb7-6nzvm               running
> # ~/.kube kubectl delete pod  complete-prodcution-e2e-deployment-59f8c44c9b-x78b6
Argo CD UI 
Applications 
Delete application 
Are you sure you want to delete the application complete-production-e2e-pipeline ?
Please type  complete-production-e2e-pipeline to confirm the deletion of the resource 
complete-production-e2e-pipeline
Ok

Argo CD UI 
Applications 
complete-production-e2e-pipeline - DELETE 

Shell Terminal 3 
> # ~/.kube clear
> # ~/.kube kubectl get pods -A
NAMESPACE NAME READY STATUS RESTARTS AGE
kube-system                            running
kube-system                            running
kube-system                            running 
…

Argo CD UI 
Applications 

No applications available to you just yet
Create new application to start managing resources in your cluster 
CREATE APPLICATION 

General
Application Name - complete-production-e2e-pipeline
Project Name - default
SYNC POLICE - Automatic 
PRUNE SOURCES
SELF HEAL
Source 
Repository URL - https://github.com/dmancloud/gitops-complete-prodcution-e2e-pipeline
Revision - HEAD
Path - ./
DESTINATION
Cluster URL - https://192.168.1.20:6443
Namespace - default
CREATE

complete-production-e2e-pipeline
Applications complete-production-e2e-pipeline

Shell Terminal 3
> # ~/.kube kubectl get pods -A
NAMESPACE NAME READY STATUS RESTARTS AGE  
kube-system                            running
kube-system                            running
kube-system                            running 
…

Shell Terminal 3 
> # ~/.kube clear
> # ~/.kube kubectl get pods -A
NAMESPACE NAME                                                                                   READY STATUS REST ARTS AGE  
default             complete-prodcution-e2e-deployment-84cdbd9fb7-h9bdx   0/1        running
default             complete-production-e2e-deployment-84cdbd9fb7-vbfc7                 running
> # ~/.kube

Deployment and Service Manifest Configured 

Shell Terminal 2 

> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main clear
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main cat deployment.yaml

apiVersion: apps/v1
kind: deployment
metadata:
   name: complete-production-e2e-deployment
spec:
   replicas: 2
   selector:
      matchLabels:
      app: complete-production-e2e-app
   templates:
      metadata:
         labels:
            app: complete-production-e2e-app
      spec: 
         containers:
             - name: complete-production-e2e-app
               image: dmancloud/complete-production-e2e-pipeline: 1.0.0-10
               resources: 
                  limits:  
                     memory: “256Mi”
                     cpu: “500m”
                ports:  
containerPort: 8080
- - -

apiVersion: apps/v1
kind: deployment
metadata:
   name: complete-production-e2e-deployment
spec:
   replicas: 2
   selector:
      matchLabels:
      app: complete-production-e2e-app
   templates:
      metadata:
         labels:
            app: complete-production-e2e-app
      spec: 
         containers:
             - name: complete-production-e2e-app
               image: dmancloud/complete-production-e2e-pipeline: 1.0.0-11
               resources: 
                  limits:  
                     memory: “256Mi”
                     cpu: “500m”
                ports:  
containerPort: 8080

Visual Studio Code
Jenkinsfile - https://github.com/dmancloud/complete-prodcution-e2e-pipeline
pipeline{
              agent 
                     label “jenkins-agent”
              tools{
                     jdk ‘java17’
                     maven ‘Maven3’
              }
              environment {
                     APP_NAME = “complete-production-e2e-pipeline”
                     RELEASE = “1.0.0”
                     DOCKER_USER = “dmancloud”
                     DOCKER_PASS = ‘dockerhub’
                     IMAGE_NAME = “${DOCKER_USER}” + “/” + “${APP_NAME}
                     IMAGE_TAG = “${RELEASE}-$(BUILD_NUMBER)”
              }
              stages{ 
                     stage(“Cleanup Workspace”){
                            steps{  
                                   cleanWs{} ( Build Function that comes with Jenkins ) 
                            }
                     }
      
                     stage(“Checkout from SCM”){
                            steps{  
                                 git branch: ‘main’, credentialsId: ‘github’, url: ‘https://github.com/dmancloud/complete-prodcution-e2e-pipeline
’
                            }
                     }

                     stage(“Build Application”){
                            steps{  
                                   sh “mvn clean package”   
                            }
                     }

                    stage(“Test Application”){
                            steps{  
                                   sh “mvn test ”    
                            }
                     }

                      stage(“Sonarqube Analysis”){
                            steps{  
                                   script {
                                          withSonarQubeEnv(credentialsId: ‘jenkins-sonarqube-token’) {
                                                 sh “mvn sonar:sonar”    
                                          }
                                   } 
                            }
                     }
               
                     stage(“Quality Gate”){
                            steps{  
                                   script {
                                          waitForQualityGate abortPipeline: false, credentialsId: ‘jenkins-sonarqube-token’                                          
                                   } 
                            }
                     }

                     stage(“Build & Push Docker Image”){
                            steps{  
                                   script {
                                          docker.withRegistry (‘’, Docker_PASS) {
                                                docker_image = docker.build “${IMAGE_NAME}”
                                          }          
                                         
                                          docker.withRegistry(‘’, Docker_PASS) {
                                                        docker_image.push(“${IMAGE_TAG}”)
                                                        docker_image.push(‘latest’)
                                                 }      
                                          }                          
                                   } 
                            }
                     }

                     stage(“Trigger CD Pipeline”){
                            steps{  
                                   script {
                                           sh “curl -v -k - -user admin:${JENKINS_API_TOKEN} -X POST -H ‘cache-control: no-cache’ -H ‘content-type: application/x-www-form-urlencoded’ - -data ‘IMAGE_TAG=${IMAGE_TAG}’ ‘https://jenkins.dev.dman.cloud/job/gitops-complete-pipeline/buildwithparameters?/token=gitops-token’ ”  
                                          }                          
                                   } 
                            }
                     }
               }
       }

Jenkins UI 
Dashboard
New Item
Enter an item name - (gitops-complete-pipeline)
Ok
Configure
General 
Discard old builds ? 
Strategy - Max # of builds to keep - 2 
This project is parameterized ?
Add parameter - String parameter 
String Parameter ? 
Name ? - IMAGE_TAG
Build Triggers
Trigger builds remotely (e.g., from scripts)
Authentication Token - gitops-token
Pipeline 
Definition - pipeline script from SCM
SCM - git
Repositories ?
Repository URL ? - https://github.com/dmancloud/gitops-complete-prodcution-e2e-pipeline
Credentials ? - dmancloud******(github)
Branches to build ? 
Branche Specifier ? (blank for ‘any’) ? - ./main
Script Path ? - Jenkinsfile
Apply
Save

Shell Terminal 2 

> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main clear
> o ~/Pr/dm/gitops-complete-production-e2e-pipeline > on o ( main branch symbol ) main code .

Visual Studio Code
Jenkinsfile - https://github.com/dmancloud/gitops-complete-prodcution-e2e-pipeline
//Jenkins UI
pipeline{
              agent 
                     label “jenkins-agent”
              }
//Application Name
              environment {
                     APP_NAME = “complete-production-e2e-pipeline”
}
              stages{ 
//Cleaning up the workspace
                     stage(“Cleanup Workspace”){
                            steps{  
                                   cleanWs() ( Build Function that comes with Jenkins ) 
                            }
                     }
      //Checking the github repository for the manifest files 
                     stage(“Checkout from SCM”){
                            steps{  
                                 git branch: ‘main’, credentialsId: ‘github’, url: ‘https://github.com/dmancloud/complete-prodcution-e2e-pipeline
’
                            }
                     }
//Modify the deployment
                     stage(“Update the deployment tags”){
                            steps{  
                                   sh “““ 
                                          cat deployment yaml
                                          sed -i ’s/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g’ deployment yaml
                                         cat deployment yaml
                                    ””””
                            }
                     }
//Push the deployment file to git
                    stage(“Push the changed deployment file to Git”){
                            steps{  
                                   sh “““
                                          git config - -global user.name “dmancloud”
                                          git config - -global user.email “dinesh@dmancloud”
                                          git add deployment.yaml
                                          git commit -m “Updated Deployment Manifest”
                                       ”””
                                   withCredentials([gitUsernamePassword(credentialsId: ‘github’, gitToolName: ‘Default’)]) {
                                          sh “git push https://github.com/dmancloud/gitops-complete-production-e2e-pipeline main”
                                          }                          
                                   } 
                            }
                     }
               }
       }

Visual Studio Code
Jenkinsfile    
pipeline{
              agent 
                     label “jenkins-agent”
              tools{
                     jdk ‘java17’
                     maven ‘Maven3’
              }
              environment {
                     APP_NAME = “complete-production-e2e-pipeline”
                     RELEASE = “1.0.0”
                     DOCKER_USER = “dmancloud”
                     DOCKER_PASS = ‘dockerhub’
                     IMAGE_NAME = “${DOCKER_USER}” + “/” + “${APP_NAME}
                     IMAGE_TAG = “${RELEASE}-$(BUILD_NUMBER)”
              }
              stages{ 
                     stage(“Cleanup Workspace”){
                            steps{  
                                   cleanWs{} ( Build Function that comes with Jenkins ) 
                            }
                     }
      
                     stage(“Checkout from SCM”){
                            steps{  
                                 git branch: ‘main’, credentialsId: ‘github’, url: ‘https://github.com/dmancloud/complete-prodcution-e2e-pipeline
’
                            }
                     }

                     stage(“Build Application”){
                            steps{  
                                   sh “mvn clean package”   
                            }
                     }

                    stage(“Test Application”){
                            steps{  
                                   sh “mvn test ”    
                            }
                     }

                      stage(“Sonarqube Analysis”){
                            steps{  
                                   script {
                                          withSonarQubeEnv(credentialsId: ‘jenkins-sonarqube-token’) {
                                                 sh “mvn sonar:sonar”    
                                          }
                                   } 
                            }
                     }
               
                     stage(“Quality Gate”){
                            steps{  
                                   script {
                                          waitForQualityGate abortPipeline: false, credentialsId: ‘jenkins-sonarqube-token’                                          
                                   } 
                            }
                     }

                     stage(“Build & Push Docker Image”){
                            steps{  
                                   script {
                                          docker.withRegistry (‘’, Docker_PASS) {
                                                docker_image = docker.build “${IMAGE_NAME}”
                                          }          
                                         
                                          docker.withRegistry(‘’, Docker_PASS) {
                                                        docker_image.push(“${IMAGE_TAG}”)
                                                        docker_image.push(‘latest’)
                                                 }      
                                          }                          
                                   } 
                            }
                     }

                     stage(“Trigger CD Pipeline”){
                            steps{  
                                   script {
                                           sh “curl -v -k - -user admin:${JENKINS_API_TOKEN} -X POST -H ‘cache-control: no-cache’ -H ‘content-type: application/x-www-form-urlencoded’ - -data ‘IMAGE_TAG=${IMAGE_TAG}’ ‘https://jenkins.dev.dman.cloud/job/gitops-complete-pipeline/buildwithparameters?/token=gitops-token’ ”  
                                          }                          
                                   } 
                            }
                     }
               }
       }

Jenkins UI
Dinesh Mistry
Configure 
Api Token 
Add new token
JENKINS_API_TOKEN - 11c04a2c261ec5e88eb60ab6b6f4fc24a5
Add new Token 
Dashboard
Manage Jenkins 
Manage Credentials ( Configure credentials ) 
Stores Scoped to Jenkins 
Domains - Global - Add credentials
New Credentials 
Kind
Username with password - Secret text
Secret - 11c04a2c261ec5e88eb60ab6b6f4fc24a5
ID ? - JENKINS_API_TOKEN
Description ? - JENKINS_API_TOKEN
Create


Visual Studio Code
Jenkinsfile -  https://github.com/dmancloud/complete-prodcution-e2e-pipeline
pipeline{
              agent 
                     label “jenkins-agent”
              tools{
                     jdk ‘java17’
                     maven ‘Maven3’
              }
              environment {
                     APP_NAME = “complete-production-e2e-pipeline”
                     RELEASE = “1.0.0”
                     DOCKER_USER = “dmancloud”
                     DOCKER_PASS = ‘dockerhub’
                     IMAGE_NAME = “${DOCKER_USER}” + “/” + “${APP_NAME}
                     IMAGE_TAG = “${RELEASE}-$(BUILD_NUMBER)”
                     JENKINGS_API_TOKEN = credentials (‘JENKINGS_API_TOKEN’)
              }
              stages{ 
                     stage(“Cleanup Workspace”){
                            steps{  
                                   cleanWs{} ( Build Function that comes with Jenkins ) 
                            }
                     }
      
                     stage(“Checkout from SCM”){
                            steps{  
                                 git branch: ‘main’, credentialsId: ‘github’, url: ‘https://github.com/dmancloud/complete-prodcution-e2e-pipeline
’
                            }
                     }

                     stage(“Build Application”){
                            steps{  
                                   sh “mvn clean package”   
                            }
                     }

                    stage(“Test Application”){
                            steps{  
                                   sh “mvn test ”    
                            }
                     }

                      stage(“Sonarqube Analysis”){
                            steps{  
                                   script {
                                          withSonarQubeEnv(credentialsId: ‘jenkins-sonarqube-token’) {
                                                 sh “mvn sonar:sonar”    
                                          }
                                   } 
                            }
                     }
               
                     stage(“Quality Gate”){
                            steps{  
                                   script {
                                          waitForQualityGate abortPipeline: false, credentialsId: ‘jenkins-sonarqube-token’                                          
                                   } 
                            }
                     }

                     stage(“Build & Push Docker Image”){
                            steps{  
                                   script {
                                          docker.withRegistry (‘’, Docker_PASS) {
                                                docker_image = docker.build “${IMAGE_NAME}”
                                          }          
                                         
                                          docker.withRegistry(‘’, Docker_PASS) {
                                                        docker_image.push(“${IMAGE_TAG}”)
                                                        docker_image.push(‘latest’)
                                                 }      
                                          }                          
                                   } 
                            }
                     }

                     stage(“Trigger CD Pipeline”){
                            steps{  
                                   script {
                                           sh “curl -v -k - -user admin:${JENKINS_API_TOKEN} -X POST -H ‘cache-control: no-cache’ -H ‘content-type: application/x-www-form-urlencoded’ - -data ‘IMAGE_TAG=${IMAGE_TAG}’ ‘https://jenkins.dev.dman.cloud/job/gitops-complete-pipeline/buildwithparameters?/token=gitops-token’ ”  
                                          }                          
                                   } 
                            }
                     }
               }
       }

Jenkins UI
Dashboard
complete-production-e2e-pipeline
Pipeline complete-production-e2e-pipeline
Build Now

Stage View
Build Now


                                   Declarative   Declarative:   Cleanup       Checkout   Build           Test 
                                   Checkout      Tool Install     Workspace  from SCM  Application                                                  SCM                           SCM

Application   Quality   Build and Push 
Analysis       Gate       Docker Image 

2s                               123ms           273ms           3s               13s             7s            14s
                                   3s                 140ms            305ms        3s               15s          7s

#12                             615ms           91ms             193ms        3s               8s             7s
11s               210ms   42s

#11                             765ms           101ms           186ms         3s               9s            7s
11s               236ms    42s

#10                             765ms           117ms           219ms         4s               8s            7s
10s               272ms    54s
       
#9 

#8                155ms

#7                              
April 14 tree commits 3s                 183ms            355ms         2s
11:50                 
#3 
Apr 14
11:14 No Changes     1s 
Average stage times  6s                  40s                373ms         3s              22s            7s      
#2
Apr 14   No Changes 6s                  40s                373ms         3s              22s            7s
11:11
#1
Apr 14   No Changes 
11:09

Jenkins UI
Dashboard
gitops-complete-pipeline
Pipeline gitops-production-pipeline
Stage View
No data available. This pipeline has not ye run

Shell Terminal 2 
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main ?1 ls
Jenkinsfile    Readme.md    deployment.yaml    service.yaml 
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main git status
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main +1 git add .
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main +1 git commit - m “added Jenkinsfile”
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main +1 git push origin main 
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main cd ../
> # ~/Pr/dmancloud ls
complete-production-e2e-pipeline    gitops-complete-production-e2e-pipeline 
> # ~/Pr/dmancloud cd complete-production-e2e-pipeline 
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !1 git status
show - -show various types of objects 
status - -show the working tree status
switch - -switch branches 
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !1 clear
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !1 git diff jenkinsfile
…
    APP_NAME = “complete-production-e2e-pipeline”
                     DOCKER_PASS = ‘dockerhub’
                     IMAGE_NAME = “${DOCKER_USER}” + “/” + “${APP_NAME}
                     IMAGE_TAG = “${RELEASE}-$(BUILD_NUMBER)”
                     APP_NAME = “complete-production-e2e-pipeline”
                     JENKINGS_API_TOKEN = “${JENKINGS_API_TOKEN}”
…

Shell Terminal 2
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !1 git add .
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main +1 git commit -m “added CD stage”
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main +1 git push origin main 
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main

Jenkins UI
Dashboard
complete-production-e2e-pipeline
Pipeline complete-production-e2e-pipeline
Build Now

Stage View
Build Now


                                   Declarative   Declarative:   Cleanup       Checkout   Build           Test 
                                   Checkout      Tool Install     Workspace  from SCM  Application                                                  SCM                           SCM

Application   Quality   Build and Push 
Analysis       Gate       Docker Image 

2s                               123ms           273ms           3s               13s             7s            14s
                                   3s                 140ms            305ms        3s               15s          7s

#13                          

#12                             615ms           91ms             193ms        3s               8s             7s
11s               210ms   42s

#11                             765ms           101ms           186ms         3s               9s            7s
11s               236ms    42s

#10                             765ms           117ms           219ms         4s               8s            7s
10s               272ms    54s
       
#9 

#8                155ms

#7                              
April 14 tree commits 3s                 183ms            355ms         2s
11:50                 
#3 
Apr 14
11:14 No Changes     1s 
Average stage times  6s                  40s                373ms         3s              22s            7s      
#2
Apr 14   No Changes 6s                  40s                373ms         3s              22s            7s
11:11
#1
Apr 14   No Changes 
11:09

Shell Terminal 2 

> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main status
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main git add .
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main +1 git commit -m “remove duplicate ENV”
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main +1 git push origin main 
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main 

Jenkins UI
Dashboard
complete-production-e2e-pipeline
Pipeline complete-production-e2e-pipeline
Build Now

Stage View
Build Now


                                   Declarative   Declarative:   Cleanup       Checkout   Build           Test 
                                   Checkout      Tool Install     Workspace  from SCM  Application                                                  SCM                           SCM

Application   Quality   Build and Push   Trigger CD
Analysis       Gate       Docker Image     Pipeline

2s                               123ms           273ms           3s               13s             7s            14s
                                   3s                 140ms            305ms        3s               15s          7s
 
#14                             732ms           97ms             194ms         3s               9s           7s
10s               236ms   45s                       476ms
#13                         

#12                             615ms           91ms             193ms        3s               8s             7s
11s               210ms   42s

#11                             765ms           101ms           186ms         3s               9s            7s
11s               236ms    42s

#10                             765ms           117ms           219ms         4s               8s            7s
10s               272ms    54s
       
#9 

#8                155ms

#7                              
April 14 tree commits 3s                 183ms            355ms         2s
11:50                 
#3 
Apr 14
11:14 No Changes     1s 
Average stage times  6s                  40s                373ms         3s              22s            7s      
#2
Apr 14   No Changes 6s                  40s                373ms         3s              22s            7s
11:11
#1
Apr 14   No Changes 
11:09

Jenkins UI
Dashboard
complete-production-e2e-pipeline
Pipeline complete-pipeline
Build Now

Visual Studio Code
Jenkinsfile -  https://github.com/dmancloud/complete-prodcution-e2e-pipeline
pipeline{
              agent 
                     label “jenkins-agent”
              tools{
                     jdk ‘java17’
                     maven ‘Maven3’
              }
              environment {
                     APP_NAME = “complete-production-e2e-pipeline”
                     RELEASE = “1.0.0”
                     DOCKER_USER = “dmancloud”
                     DOCKER_PASS = ‘dockerhub’
                     IMAGE_NAME = “${DOCKER_USER}” + “/” + “${APP_NAME}
                     IMAGE_TAG = “${RELEASE}-$(BUILD_NUMBER)”
                     JENKINGS_API_TOKEN = credentials (‘JENKINGS_API_TOKEN’)
              }
              stages{ 
                     stage(“Cleanup Workspace”){
                            steps{  
                                   cleanWs{} ( Build Function that comes with Jenkins ) 
                            }
                     }
      
                     stage(“Checkout from SCM”){
                            steps{  
                                 git branch: ‘main’, credentialsId: ‘github’, url: ‘https://github.com/dmancloud/complete-prodcution-e2e-pipeline
’
                            }
                     }

                     stage(“Build Application”){
                            steps{  
                                   sh “mvn clean package”   
                            }
                     }

                    stage(“Test Application”){
                            steps{  
                                   sh “mvn test ”    
                            }
                     }

                      stage(“Sonarqube Analysis”){
                            steps{  
                                   script {
                                          withSonarQubeEnv(credentialsId: ‘jenkins-sonarqube-token’) {
                                                 sh “mvn sonar:sonar”    
                                          }
                                   } 
                            }
                     }
               
                     stage(“Quality Gate”){
                            steps{  
                                   script {
                                          waitForQualityGate abortPipeline: false, credentialsId: ‘jenkins-sonarqube-token’                                          
                                   } 
                            }
                     }

                     stage(“Build & Push Docker Image”){
                            steps{  
                                   script {
                                          docker.withRegistry (‘’, Docker_PASS) {
                                                docker_image = docker.build “${IMAGE_NAME}”
                                          }          
                                         
                                          docker.withRegistry(‘’, Docker_PASS) {
                                                        docker_image.push(“${IMAGE_TAG}”)
                                                        docker_image.push(‘latest’)
                                                 }      
                                          }                          
                                   } 
                            }
                     }

                     stage(“Trigger CD Pipeline”){
                            steps{  
                                   script {
                                           sh “curl -v -k - -user admin:${JENKINS_API_TOKEN} -X POST -H ‘cache-control: no-cache’ -H ‘content-type: application/x-www-form-urlencoded’ - -data ‘IMAGE_TAG=${IMAGE_TAG}’ ‘https://jenkins.dev.dman.cloud/job/gitops-complete-pipeline/buildwithparameters?/token=gitops-token’ ”  
                                          }                          
                                   } 
                            }
                     }
               }
       }

Shell Terminal 2

> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main clear
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !1 git status
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main !1 git add .
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main +1 git commit -m “fixed type in Jenkinsfile”
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main +1 git push origin main 
> o ~/Pr/dm/complete-production-e2e-pipeline > on o ( main branch symbol ) main

Jenkins UI
Dashboard
complete-production-e2e-pipeline
Pipeline complete-pipeline
Build Now

Stage View
Build Now


                                   Declarative   Declarative:   Cleanup       Checkout   Build           Test 
                                   Checkout      Tool Install     Workspace  from SCM  Application                                                  SCM                           SCM

Application   Quality   Build and Push   Trigger CD
Analysis       Gate       Docker Image     Pipeline

2s                               123ms           273ms           3s               13s             7s            14s
                                   3s                 140ms            305ms        3s               15s          7s
 
#15                             757ms           108ms           211ms         3s               9s           7s 
11s                250ms   

#14                             732ms           97ms             194ms         3s               9s           7s
10s               236ms   45s                       476ms
#13                         

#12                             615ms           91ms             193ms        3s               8s             7s
11s               210ms   42s

#11                             765ms           101ms           186ms         3s               9s            7s
11s               236ms    42s

#10                             765ms           117ms           219ms         4s               8s            7s
10s               272ms    54s
       
#9 

#8                155ms

#7                              
April 14 tree commits 3s                 183ms            355ms         2s
11:50                 
#3 
Apr 14
11:14 No Changes     1s 
Average stage times  6s                  40s                373ms         3s              22s            7s      
#2
Apr 14   No Changes 6s                  40s                373ms         3s              22s            7s
11:11
#1
Apr 14   No Changes 
11:09

Jenkins UI
Dashboard
gitops-complete-pipeline
Pipeline gitops-complete-pipeline

Stage View
                      
                     Declarative:   Cleanup       Checkout    Update the     Push the
                     Checkout       Workspace  from SCM    Deployment   Changed 
                     SCM                                                      Tags               Deployment
                                                                                                         file to Git

#1                  555ms           97ms            562ms        319ms            1s

Argo CD
Applications
complete-production-e2e-pipeline 
SYNC
SYNCHRONIZE
complete-production-e2e-pipeline 
 


