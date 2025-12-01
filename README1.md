**Pushing multi-folder project into public repository (by teacher).**

create a multi folder using cmd

cd /c/Users/"Riya Mehta"/Downloads
mkdir multiFolderProj
cd multiFolderProj
mkdir src assets database docs
echo .>README.md
echo .>src\app.java
echo .>docs\notes.txt

commands in git bash

cd /c/Users/"Riya Mehta"/Downloads/multiFolderProj
pwd
git init
git add .
git commit -m "Initial commit for multi folder project"
git remote add origin https://github.com/RiyaMehta1/weeTwoSELab.git
IF REMOTE ALREADY EXISTS
git remote set-url origin https://github.com/RiyaMehta1/weeTwoSELab.git
git branch -M main
git push -u origin main

**To resolve conflicts when collaborating on same part of code**

cd /c/Users/"Riya Mehta"/Downloads
mkdir conflictPracticeLab
cd conflictPracticeLab
mkdir src assets docs database
touch README.md

git init
notepad README.md

This is my project
conflict line
End of file

git add .
git commit -m "Initial commit with base conflict line"
git remote add origin https://github.com/RiyaMehta1/weekThreeSELab
git branch -M main
git push -u origin main

git branch collab-1
git branch collab-2

git checkout collab-1
notepad README.md

conflict line -> conflict line updated by FIRST collab

git add README.md
git commit -m "First collaborator updates conflict line"
git push origin collab-1

git checkout collab-2
notepad README.md

conflict line -> conflict line updated by SECOND collab

git add README.md
git commit -m "Second collaborator updates conflict line"
git push origin collab-2

git checkout main
git merge collab-2

git push origin main

git merge collab-1
conflict
notepad README.md
resolve conflict

git add README.md
git commit -m "Resolve conflict by keeping both change"
git push origin main

**To create and apply patch**

SENDER SIDE

cd /c/Users/"Riya Mehta"/Downloads/multiFolderProject
git status
echo "Patch practice line from sender" >> README.md
git add README.md
git commit -m "sender: added a line to README for patch demo"
git push origin main

git log --oneline -n 5
git format-patch -1 a1b2c3d
ls

RECEIVER SIDE

cd /c/Users/"Riya Mehta"/Downloads
git clone https://github.com/RiyaMehta1/weekTwoSELab.git receiver-repo
cd receiver-repo
git checkout main
git pull origin main
cp ../multiFolderProj/0001-*.patch ./
git am 0001-*.patch
git log --oneline -n 5
git push origin main

**Working with Docker file**

A Docker file is a text file with instructions to create a custom Docker image.

Step 1: Set Up Your Folder
1. Windows:
o Create a folder like C:\DockerProjects\Redis.
o Open Git Bash and navigate to the folder:
cd /c/DockerProjects/Redis

Step 2: Write the Dockerfile
1. Inside the folder, create a file named Dockerfile (no extension).
2. Add the following content:
FROM redis:latest
CMD ["redis-server"]

Docker Commands (Step-by-step):

1. docker build -t redisnew  .
What it does:
 This creates (builds) a Docker image using the recipe (Dockerfile) in the current
folder (.).
 -t redisnew: Gives the image a name/tag ("redisnew"), so you can find it easily.
2. docker run --name myredisnew -d redisnew
What it does:
 Starts a new container (mini computer) from the redisnew image.
 --name myredisnew: Names the container "myredisnew" so it’s easy to identify.
 -d: Runs the container in the background.
3. docker ps
What it does:
 Shows a list of containers that are running right now.
4. docker stop myredisnew
What it does:
 Stops the container named "myredisnew" (like turning off a computer).
5. docker login
What it does:
 Logs you into your Docker Hub account, so you can upload images.
6. docker ps -a
What it does:
 Shows a list of all containers, including stopped ones.
7. docker commit 0e993d2009a1 budarajumadhurika/redis1
// Note 0e993d2009a1 is container id of myredisnew
What it does:
 Takes a snapshot (saves changes) of the container with ID 0e993d2009a1 and creates a
new image called budarajumadhurika/redis1.
8. docker images
What it does:
 Lists all images saved on your system. // will show image of budarajumadhurika/redis1
9. docker push budarajumadhurika/redis1
What it does:
 Uploads the image budarajumadhurika/redis1 to Docker Hub, so others can download
it.
10. docker rm 0e993d2009a1  // container id of myredisnew
What it does:
 Deletes the container with ID 0e993d2009a1 of myredisnew.
11. docker rmi budarajumadhurika/redis1
What it does:
 Deletes the image budarajumadhurika/redis1 from your system.
12. docker ps -a
What it does:
Shows all containers again to confirm changes.
13. docker logout
What it does:
 Logs you out of Docker Hub.
14. docker pull budarajumadhurika/redis1
What it does:
 Downloads the image budarajumadhurika/redis1 from Docker Hub.
15. docker run --name myredis -d budarajumadhurika/redis1
What it does:
 Starts a new container using the image budarajumadhurika/redis1.
16. docker exec -it myredis redis-cli
What it does:
 Opens the Redis command-line interface (like a terminal) inside the running container
myredis.
17. SET name "Abcdef"
What it does:
 Saves a key-value pair in Redis (key = name, value = Abcdef).
18. GET name
What it does:
 Retrieves the value of the key name from Redis (it will return "Abcdef").
19. exit
What it does:
 Exits the Redis CLI.
20. docker ps -a
What it does:
 Shows all containers again to check their status.
21. docker stop myredis
What it does:
 Stops the container myredis.
22. docker rm 50a6e4a9c326
What it does:
 Deletes the container with ID 50a6e4a9c326.
23. docker images
What it does:
 Lists all images again to confirm which ones remain.
24. docker rmi budarajumadhurika/redis1
What it does:
 Deletes the image budarajumadhurika/redis1 again.
Step 3: Remove Login Credentials (Optional)
If you no longer need to be logged in, you can log out:
docker logout
What It Does:
 Logs you out from Docker Hub and removes your stored credentials.

**DOCKER COMPOSE**

1.	folder (dockerProject - > file (docker-compose.yaml))
2.	docker-compose.yaml content

version: '3.8'

services:
  wordpress:
    image: wordpress:latest
    restart: always
    ports:
      - "8082:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wp_user
      WORDPRESS_DB_PASSWORD: wp_pass
      WORDPRESS_DB_NAME: wordpress_db
    depends_on:
      - db
    volumes:
      - wordpress_data:/var/www/html

  db:
    image: mysql:5.7
    container_name: mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: wordpress_db
      MYSQL_USER: wp_user
      MYSQL_PASSWORD: wp_pass
    volumes:
      - db_data:/var/lib/mysql

volumes:
  wordpress_data:
  db_data:

3. docker-compose up -d
4. docker ps
5. http://localhost:8082
6. docker-compose up --scale wordpress=2 -d
7. docker-compose logs -f
8. docker ps
9. docker exec -it mysql mysql -u wp_user -p
    (password = docker-compose.yaml mysql       user_password = wp_pass)
10. show databases;
11. use wordpress_db
12. show tables;
13. select user_login,user_email from wp_users;
14. exit;
15. docker-compose down
16.docker-compose stop

**Maven java and Maven web project using eclipse and pushing into GITHUB**

**Creation of Maven Java Project Procedure Steps:**

Step1: Open Eclipse IDE for Enterprise JAVA and WEB developers.
Step2: Click on File->New->Maven Project
Step3: Type quickstart in Filter and wait for artifacts to load.
Step4: Select org.apache.maven.archetypes   maven-archetype-quickstart 1.4 and click on next
Step5: Mention TeamID: as SE, ArtifactID: MavenJava and click on next
Step6: Now in Console we can see all the artifacts are grouped and console is waiting as Y: Now click Y and finally SE.MavenJava gets ready by combining all the artifacts.
Step7: We can see our MavenJava project ready with its own pom.xml file in the Project Explorer tool bar menu present at the left side.
Step8: Now click on src/main/java->SE.MavenJava->App.java where we can find a code sample for HELLO WORLD.
Step9: Now right click on App.java->Run as->Maven Clean
Step10: Now right click on App.java->Run as->Maven Install
Step11: Now right click on App.java->Run as->Maven Test
Step12: Now right click on App.java->Run as->Maven Build
Step13: In Goals, Type “clean install test”, click on apply next click on Run, for building the MavenJava project.
Step14: Every phase is checked and BUILD SUCCESS message can be seen in the console. 
Step15: Now right click on App.java->Run as->Java Application, click on OK.
Step16: Hello world is shown as output for MavenJava Project.

**Creation of Maven Web Project Procedure Steps**:

Step1: Open Eclipse IDE for Enterprise JAVA and WEB developers.
Step2: Click on File->New->Maven Project
Step3: Type webapp in Filter and wait for artifacts to load.
Step4: Select org.apache.maven.archetypes   maven-archetype-webapp 1.4 and click on next
Step5: Mention TeamID: as SE, ArtifactID: MavenWeb and click on next
Step6: Now in Console we can see all the artifacts are grouped and console is waiting as Y: Now click Y and finally SE.MavenWeb gets ready by combining all the artifacts.
Step7: We can see our MavenWeb project ready with its own pom.xml file in the Project Explorer tool bar menu present at the left side.
Step8: Now click on MavenWeb->src->main->Webapp->index.jsp where we can find a code sample for HELLO WORLD.
Step9: Now open browser and open mvnrepository.com
Step10: Search for JAVA servlet API and click on the latest version.
Step11: Copy the dependency code of the servlet. And paste it in MavenWeb’s pom.xml under target folder.
Step12: Now click on window in the menu bar, click on showview->servers.
Step13: Add servers by clicking on “No servers available”.
Step14: Select Tomcat v9.0 server/ Tomcat v11.0 server in the server type Click on next, select Apache Tomcat v9.0(optional), Click Apply and close.
Step15: Now we can find tomcat server attached to the project in server menu of the console. Duble click on it where the tomcat configuaration page appears.
Step16: select “Use Tomcat Installation”(which Is 2nd option) in Server locations, and modify the port numbers to “0” for admin port and “8085” for Https/1.1 and close the tab.
Step17: Double click “servers” folder present at the left side in project explorer, click on Tomcat v9.0 server at localhost-config->tomcat-users.xml, scroll to the end and paste the following exactly above </tomcat-users> tag and close the tab.

role rolename="admin-gui,manager-gui ,manager-script,manager-jmx,manager-status"/><user password="1234" roles="manager-gui, admin-gui ,manager-script" username="admin"/> 


Step18: Now right click on index.jsp ->Run as->Maven Clean
Step19: Now right click on index.jsp ->Run as->Maven Install
Step20: Now right click on index.jsp ->Run as->Maven Test
Step21: Now right click on index.jsp ->Run as->Maven Build
Step22: In Goals, Type “clean install test”, click on apply next click on Run, for building the MavenWeb project.
Step23: Every phase is checked and BUILD SUCCESS message can be seen in the console. 
Step24: Now right click on index.jsp ->Run as->Run on server, click on OK.
Step25: Select choose an existing server and click on finish.
Step26: Hello world Webpage is shown as output for MavenWeb Project.

**Steps to Push the Maven Projects to GIT**:

Java Project Push Procedure Steps:
Step1: Create two repositories for MavenJava and MavenWeb projects in your GitHUB account.
Step2: In eclipse right click on MavenJava folder present in project explorer and select show in local terminal option->GitBash.
Step3: execute: git init-> git branch –M main.
Step4: Now add your repository to the MavenJava using, git remote add origin <git java repository url> command.
Step5: execute: git add . -> git commit –m “Maven java push” -> git push –u origin main
Step6: Now refresh your GitHub account and we can see our MavenJava files are pushed to MavenJava repository in your GitHub account.

Web Project Push Procedure Steps:
Step1: In eclipse right click on MavenWeb folder present in project explorer and select show in local terminal option->GitBash.
Step2: execute: git init-> git branch –M main.
Step3: Now add your repository to the MavenWeb using, git remote add origin <git Web repository url> command.
Step4: Push your web project same as Java to GitHub. 
Step5: Now refresh your GitHub account and we can see our MavenWeb files are pushed to MaveWeb repository in your GitHub account.

**Week 8: Jenkins Automation**

Hands-on practice on manual creation of Jenkins pipeline using Maven projects from Github 
Create the job and build the pipeline for maven-java and maven-web project.
Questions on Jenkins
Upload the Screenshots.

Steps for MavenJava Automation:
**Maven Java Automation Steps:**
 Step 1: Open Jenkins (localhost:8080)
   	 ├── Click on "New Item" (left side menu
Step 2: Create Freestyle Project (e.g., MavenJava_Build)
        	├── Enter project name (e.g., MavenJava_Build)
        	├── Click "OK"
└── Configure the project:
            		├── Description: "Java Build demo"
            		├── Source Code Management:
            			└── Git repository URL: [GitMavenJava repo URL]
            		├── Branches to build: */Main   or  */master
  		└── Build Steps:
               	     ├── Add Build Step -> "Invoke top-level Maven targets"
                  		└── Maven version: MAVEN_HOME
                 		└── Goals: clean
                	├── Add Build Step -> "Invoke top-level Maven targets"
                		└── Maven version: MAVEN_HOME
                		└── Goals: install
                	└── Post-build Actions:
                    	       ├── Add Post Build Action -> "Archive the artifacts"
                    			└── Files to archive: **/*
                    	     ├── Add Post Build Action -> "Build other projects"
                    			└── Projects to build: MavenJava_Test
                    			└── Trigger: Only if build is stable
                    	     └── Apply and Save


    └── Step 3: Create Freestyle Project (e.g., MavenJava_Test)
        	├── Enter project name (e.g., MavenJava_Test)
        	├── Click "OK"
              └── Configure the project:
             ├── Description: "Test demo"
             ├── Build Environment:
            		└── Check: "Delete the workspace before build starts"
            ├── Add Build Step -> "Copy artifacts from another project"
            		└── Project name: MavenJava_Build
            		└── Build: Stable build only  // tick at this
            		└── Artifacts to copy: **/*
            ├── Add Build Step -> "Invoke top-level Maven targets"
            		└── Maven version: MAVEN_HOME
            		└── Goals: test
            		└── Post-build Actions:
                ├── Add Post Build Action -> "Archive the artifacts"
                	└── Files to archive: **/*
                	└── Apply and Save

    └── Step 4: Create Pipeline View for Maven Java project
        ├── Click "+" beside "All" on the dashboard
        ├── Enter name: MavenJava_Pipeline
        ├── Select "Build pipeline view"         // tick here
         |--- create
        └── Pipeline Flow:
            ├── Layout: Based on upstream/downstream relationship
            ├── Initial job: MavenJava_Build
            └── Apply and Save OK

    └── Step 5: Run the Pipeline and Check Output
        ├── Click on the trigger to run the pipeline
        ├── click on the small black box to open the console to check if the build is success
            Note : 
If build is success and the test project is also automatically triggered with name       
                      “MavenJava_Test”
The pipeline is successful if it is in green color as shown ->check the console of the test project
The test project is successful and all the artifacts are archived successfully

**II. Maven Web Automation Steps:**
└── Step 1: Open Jenkins (localhost:8080)
    ├── Click on "New Item" (left side menu)
    
    └── Step 2: Create Freestyle Project (e.g., MavenWeb_Build)
        ├── Enter project name (e.g., MavenWeb_Build)
        ├── Click "OK"
        └── Configure the project:
            ├── Description: "Web Build demo"
            ├── Source Code Management:
            		└── Git repository URL: [GitMavenWeb repo URL]
            ├── Branches to build: */Main or master
            └── Build Steps:
                ├── Add Build Step -> "Invoke top-level Maven targets"
                	└── Maven version: MAVEN_HOME
                	 └── Goals: clean
                ├── Add Build Step -> "Invoke top-level Maven targets"
                	└── Maven version: MAVEN_HOME
                	└── Goals: install
                └── Post-build Actions:
                    ├── Add Post Build Action -> "Archive the artifacts"
                   	 └── Files to archive: **/*
                    ├── Add Post Build Action -> "Build other projects"
                    	└── Projects to build: MavenWeb_Test
                    	└── Trigger: Only if build is stable
                    └── Apply and Save

    └── Step 3: Create Freestyle Project (e.g., MavenWeb_Test)
        ├── Enter project name (e.g., MavenWeb_Test)
        ├── Click "OK"
        └── Configure the project:
            ├── Description: "Test demo"
            ├── Build Environment:
            		└── Check: "Delete the workspace before build starts"
            ├── Add Build Step -> "Copy artifacts from another project"
            		└── Project name: MavenWeb_Build
            		└── Build: Stable build only
            		└── Artifacts to copy: **/*
            ├── Add Build Step -> "Invoke top-level Maven targets"
           		└── Maven version: MAVEN_HOME
            		└── Goals: test
            └── Post-build Actions:
                ├── Add Post Build Action -> "Archive the artifacts"
                	└── Files to archive: **/*
                ├── Add Post Build Action -> "Build other projects"
                	└── Projects to build: MavenWeb_Deploy
                └── Apply and Save

    └── Step 4: Create Freestyle Project (e.g., MavenWeb_Deploy)
        ├── Enter project name (e.g., MavenWeb_Deploy)
        ├── Click "OK"
        └── Configure the project:
            ├── Description: "Web Code Deployment"
            ├── Build Environment:
            		└── Check: "Delete the workspace before build starts"
            ├── Add Build Step -> "Copy artifacts from another project"
            		└── Project name: MavenWeb_Test
            		└── Build: Stable build only
               	└── Artifacts to copy: **/*
            └── Post-build Actions:
                ├── Add Post Build Action -> "Deploy WAR/EAR to a container"
   └── WAR/EAR File: **/*.war
   └── Context path: Webpath
 └── Add container -> Tomcat 9.x remote
└── Credentials: Username: admin, Password: 1234
── Tomcat URL: https://localhost:8085/
                └── Apply and Save

    └── Step 5: Create Pipeline View for MavenWeb
        ├── Click "+" beside "All" on the dashboard
        ├── Enter name: MavenWeb_Pipeline
        ├── Select "Build pipeline view"
        └── Pipeline Flow:
            ├── Layout: Based on upstream/downstream relationship
            ├── Initial job: MavenWeb_Build
            └── Apply and Save

    └── Step 6: Run the Pipeline and Check Output
        ├── Click on the trigger “RUN” to run the pipeline
            Note: 
After Click on Run -> click on the small black box to open the console to check if the build is success
Now we see all the build has  success if it appears in green color
        ├── Open Tomcat homepage in another tab
        ├── Click on the "/webpath" option under the manager app
               Note:
 It ask for user credentials for login ,provide the credentials of tomcat.
It provide the page with out project name which is highlighted.
After clicking on our project we can see output.

**Pipeline Creation using  Script**

pipeline {
    agent any

    tools {
        // Replace 'MAVEN-HOME' with your Maven tool name configured in Jenkins
        maven 'MAVEN-HOME'
    }

    triggers {
        // Example: poll GitHub every 5 minutes — change as needed
        pollSCM('H/5 * * * *')
    }

    stages {
        stage('Checkout & Clean') {
            steps {
                bat "git clone https://github.com/RiyaMehta1/SampleMavenJavaProject.git"
                bat "cd SampleMavenJavaProject && mvn clean"
            }
        }

        stage('Build & Install') {
            steps {
                bat "cd SampleMavenJavaProject && mvn install"
            }
        }

        stage('Test') {
            steps {
                bat "cd SampleMavenJavaProject && mvn test"
            }
        }

        stage('Package') {
            steps {
                bat "cd SampleMavenJavaProject && mvn package"
            }
        }
    }
}

**Setting Up   Jenkins Email Notification Setup (Using Gmail with App Password)**

Creation of app password

1. Gmail: Enable App Password (for 2-Step Verification)
i. Go to: https://myaccount.google.com
ii. Enable 2-Step Verification
Navigate to:
Security → 2-Step Verification
Turn it ON
Complete the OTP verification process (via phone/email)
iii. Generate App Password for Jenkins
Go to:
Security → App passwords
Select:
App: Other (Custom name)
Name: Jenkins-Demo
Click Generate
Copy the 16-digit app password
Save it in a secure location (e.g., Notepad)

2.  Jenkins Plugin Installation
i. Open Jenkins Dashboard
ii. Navigate to:
Manage Jenkins → Manage Plugins
iii. Install Plugin:
Search for and install:
Email Extension Plugin

3. Configure Jenkins Global Email Settings
i. Go to:
Manage Jenkins → Configure System

A. E-mail Notification Section
Field
Value
SMTP Server
smtp.gmail.com
Use SMTP Auth
✅ Enabled
User Name
Your Gmail ID (e.g., archanareddykmit@gmail.com)
Password
Paste the 16-digit App Password
Use SSL
✅ Enabled
SMTP Port
465
Reply-To Address
Your Gmail ID (same as above)

➤ Test Configuration
Click: Test configuration by sending test e-mail
Provide a valid email address to receive a test mail
✅ Should receive email from Jenkins

B. Extended E-mail Notification Section
Field
Value
SMTP Server
smtp.gmail.com
SMTP Port
465
Use SSL
✅ Enabled
Credentials
Add Gmail ID and App Password as Jenkins credentials
Default Content Type
text/html or leave default
Default Recipients
Leave empty or provide default emails
Triggers
Select as per needs (e.g., Failure)


4.  Configure Email Notifications for a Jenkins Job
i. Go to:
Jenkins → Select a Job → Configure

ii. In the Post-build Actions section:
Click: Add post-build action → Editable Email Notification
A. Fill in the fields:
Field
Value
Project Recipient List
Add recipient email addresses (comma-separated)
Content Type
Default (text/plain) or text/html
Triggers
Select events (e.g., Failure, Success, etc.)
Attachments
(Optional) Add logs, reports, etc.


iii. Click Save

 Now your Jenkins job is set up to send email notifications based on the build status!

