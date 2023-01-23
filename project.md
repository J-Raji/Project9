# Continuous Integration Pipeline with Jenkins Server
1. Install and Configure Jenkins
**Create instance in south africa region**
![Ubuntu 20. LTS Instance](jenkins.png)
`ssh -i "jen-key.pem" ubuntu@ec2-15-228-54-237.sa-east-1.compute.amazonaws.com`
2. Install JDK
`sudo apt update`
![apt updated](apt-update.png)
`sudo apt install default-jdk-headless`
![jdk default](jdk.png)
3.Install Jenkins

`wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -`
`sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'`
`sudo apt update`
`sudo apt-get install jenkins`
![jenkins installed](jenkins-install.png)

- Make sure Jenkins is up and running
`sudo systemctl status jenkins`
![jenkins status](stat.png)

5. By default Jenkins server uses TCP port 8080 – open it by creating a new Inbound Rule in your EC2 Security Group
![Inbound rule](port.png)

- Perform initial Jenkins setup.

From your browser access http://15.228.54.237:8080

You will be prompted to provide a default admin password 
- Run
`sudo journalctl -u jenkins.service`
or
`sudo vi /var/lib/jenkins/secrets/initialAdminPassword`
 d84735f4243e4faf8ea684558518804e
![jenkins logged in](site.png)
-Setup Username admin and password as above
![jenkins user setup](setup.png)

## Step 2 – Configure Jenkins to retrieve source codes from GitHub using Webhooks
1. Enable webhooks in your GitHub repository settings
[add webhooks](https://github.com/J-Raji/Project9/settings/hooks/397986019)

2. Go to Jenkins web console, click "New Item" and create a "Freestyle project"
-Install Git Server update
![tooling github linked](git-link.png)

3. Click "Configure" your job/project and add these two configurations
Configure triggering the job from GitHub webhook:


- Archive the artifact on Post Actions
![saved and applied](post.png)