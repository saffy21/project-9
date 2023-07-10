# CREATING TOOLING WEBSITE DEPLOYMENT AUTOMATION WITH CONTINUOUS INTEGRATION

![project architecture](./images_9/Architecture_end_goal%20_of_project.png)

### Creating an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it “Jenkins”

![creating ubuntu server for Jenkins](./images_9/Ubuntu_20.04_LVM_EC2_instance_for_Jenkins.png)

### Install JDK (because Jenkins is a Java-based application)

`sudo apt update`

`sudo apt install default-jdk-headless`

## Install Jenkins

`curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null`

`echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null`

`sudo apt-get update`

`sudo apt-get install jenkins`

### confirming if Jenkins is up and running

`sudo systemctl status jenkins`

![confirming if Jenkins is running](./images_9/confirming_if_Jenkins_is_up_and_running.png)

### opening port 8080 for Jenkins

![opening port 8080 for Jenkins named server](./images_9/adding_port_8080_to_security_group.png)

[accessing Jenkins through browser](http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080)

![accessing Jenkins Page](./images_9/access_jenkins_key_in_password.png)

### Retrieving password to unlock Jenkins in browser using the code below in the Jenkins server

`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

### Creating first user on Jenkins

![creating user on Jenkins](./images_9/creating_first_user_on_jenkins.png)

![Jenkins is ready](./images_9/Jenkins_is_ready.png)

![welcome to Jenkins](./images_9/welcome_to_Jenkins.png)

## Configuring Jenkins to retrieve source codes from GitHub using Webhooks

### Enabling webhooks in your GitHub my settings

![copy https code](./images_9/copy_https_code_from_source.png)

### create a new repository in my account (destination), then import repository using https copied from source github account

### Go to settings in tooling repository in destination github account, enter the Jenkins server public ip address as shown in image then add webhook

![entering settin information](./images_9/adding_webhook_from_setting.png)

### webook addition confirmation page

![confirmation of adding webook](./images_9/webook_added.png)

###  Go to Jenkins web console, click “New Item” enter an item name and create a “Freestyle project”

![creating an item](./images_9/creating_an_item_on_jenkins.png)

### To connect your GitHub repository from Jenkins, you will need to provide its URL, you can copy from the repository itself

![copying http from github tooling repository](./images_9/copy_http_url_from_github_tooling_for_Jenkins.png)

### In the general page check git in the source code management pane and paste code in repository url and add credentials (by entering my git username and password in the fields) then click on jenkins  if you get an http 403 no valid crumb simply go to settings >configure global security > CSRF Protection > check or mark Enable proxy compatibility

![adding git repository in Jenkins](./images_9/adding_github_repository_in_Jenkins.png)

![adding credentials git username and password](./images_9/adding_username_and_password_of_git_to_jenkins.png)


### go to Build now, then find Build history down then click on the option 

![Build now](./images_9/Build_Now.png)

![Build#1](./images_9/Build%231.png)

### click on console output in the left pane to check if it was a success

![console output](./images_9/console_output.png)

## Step 3

from dashboard click on the item (project9) > configure > build triggers > check or mark GitHub hook trigger for GITScm polling

![build triggers](./images_9/Build_triggers.png)

buid environment > add post build action > archive the artifacts > type ** in the files to archive field > save

![archiving artifacts](./images_9/archiving_artifacts.png)

### Go to github > tooling repository > click on the readme file to edit the file > click edit > scroll down to the last part enter "Checking Jenkins" > commit changes

![editing readme in github](./images_9/editing_readme_in%20_github.png)

### Go to status on Jenkins and confirm settings

![status of setting](./images_9/status_of_successful_artifacts.png)

`sudo systemctl restart jenkins`

`cd /var/lib/jenkins/`

`ls`

`cd jobs`

`cd project9`

`cd builds`

`cd 2`

`cd archive`

![checking jenkins folder](./images_9/checking_Jenkins_%20Folder.png)

![checking artifacts in jenkins folder](./images_9/checking_artifacts_in%20Jenkins_folder.png)

## Step 4 – Configure Jenkins to copy files to NFS server via SSH

### first we have to in stall plugins in Jenkins... go to Dashboard > manage Jenkins > plugins > available plugins > search "Publish Over SSH" > click on install without restart button

![plugin installation complete](./images_9/plugin_installation_complete.png)

### Configure the job/project to copy artifacts over to NFS server

### Dashboard > manage jenkins > configure system > look for the "Publish Over SSH" we just downloaded configure it as shown in image below > test configuration > save
note .pem key was copied to the key field and also the NFS private ip address was copied to the host name


![publish over ssh configuration settings](./images_9/Publish_over_SSH_plugin_configuration_setting.png)

![publish over ssh configuration settings](./images_9/Publish_over_SSH_plugin_configuration_setting2.png)

### go to project > configure > build triggers > add post build actions > send build artifacts over SSH > set "source files" to ** > save

### go to > github tooling readme file > edit it by deleting "Checking Jenkins" commit changes  

`sudo chown -R nobody:nobody /mnt` in the nfs server

`sudo chmod -R 777 /mnt` in the nfs server

### go to dashboard in jenkins > project9 > build now > click on the latest build #5 (in my case) > console output

![latest build](./images_9/latest_build.png)
![console output success](./images_9/console_output_success.png)

`cd`     in the nfs server

`cd /mnt/apps`      in the nfs server

`cat /mnt/apps/README.md`      in the nfs server

![checking readme changes](./images_9/checking_changes_to_readme_file.png)

![checking readme changes](./images_9/checking_changes_to_readme_file_continued.png)

![checking readme changes](./images_9/checking_changes_to_readme_contdd.png)