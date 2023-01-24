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

-confirm build
![confirm update of project in Jenkins](build.png)

## Configure Jenkins to copy files to NFS server via SSH
1. Install "Publish Over SSH" plugin.
![Installed Publish over ssh](publish.png)
2. Configure the job/project to copy artifacts over to NFS server.
- On main dashboard select "Manage Jenkins" and choose "Configure System" menu item.

Scroll down to Publish over SSH plugin configuration section and configure it to be able to connect to your NFS server:
`ssh -i "Nfs-key.pem" ec2-user@ec2-18-231-189-79.sa-east-1.compute.amazonaws.com`

- Provide a private key (content of .pem file that you use to connect to NFS server via SSH/Putty)
    -----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAj5I0zT2hb4GLTgOncL3sDwsg0EdGtzZgDc1cELwc8eWpaxtZ
nBqKqGcRHKKcqh+vv4dU+USbzLDhzI8M2HkMKhDouE8/hwpFGcGMLa+sqtJ7LYUA
kDUoVLODPA+ZbCHi1wLfm4KMtehbIB+Cvwc2z7tUb/47AdxKt72NgOwgCHVFMzmQ
7dDqDbx2P1HgkfHXFPLC5r7fPHx3z+trzKE3njsaRDNHq51rHlA68AUEF8Fpo5Fg
2SiQbuxCqad3pi9GujW4fuI5oS8fIg7wniqS/8hyRRT6ZTxfwYS52BM07X9IXqfT
PqYOGNtUltJ8sA2lnqbB/76FeU86PSuvXQYJnQIDAQABAoIBAE9e7c/1XXUusdu8
S2oZpRIf/dEHRoHtDqcyu84IoRvd8o5i/WQ+jB9Tc3NYNrIaeGezInf3xQYhV4Nm
Jhzatq3e0TlrnlxCgjcd+CgdsaByYmSk3c3bhWNmJowit5e/GA/z57iqMK40OYSF
xxtimpu3HZQYgXii16/CnCME5ySlFFSGcA9+o5ete812/rFeBrD2y5+kMRsdvChU
jN5tsupdgJpSlwe0BY1O6urUHrSqJp/u8BQyW5IVexLOJLf2QkGmCdo3vh5KwsIG
dkl3LB7iX0yHFm/y/vsDncptrhpwRJTKejM/KHkD7gKR3Wiz1k2TDDpoO64UMQ0H
FAhcAoUCgYEAwTBCDlwy0s3AoR7BLKQvDh5PIvgYAQiDKE9jeuVJElYiCazfYYm9
DcdcupMEQ7ovatDSeiWOghNgVe0DIVKPb3Ej615vRX1OzyrM9f1uqhhjnIhue9oA
qpf6bAtZEbdQhBEABVcmy5vFFDtNP8lZZXixqsE4OnkLbNh6DIBBgHsCgYEAvkAf
a6JAo2eEJFsn85/HJrgf9PymgmjORy9pNBlt+Ci+fvd/pAI+Grpw5WYIYPGbNtVA
BWFU9/aKfxEnc/sgMG2IrepZamSFlmYjq/VEA3wjmGgeq1HFdJS+uBxrKq3o2dF1
wew1Gz59B6pP0FFEsjVH00hM49h6eCLcdh5qXscCgYEAmkolZ3yhJpUm9EcwtquF
3Tu9rksAOMsInQgShlNasadS1fFYEnlEIR4I5AWIkWLAfgm7H8yg7Sf2d4msR0+9
uJ5etpscOR5j87bWLNw0JusFmz2nJ4kroRNx8Bp8D1cdmexN3PYGyPRmSMs33eq8
V/s1wg9BDgogYtTdXOCN78MCgYEAsNYBXpZt5nStiu1/8R8uiXrTmW/NhaHNOrWC
3/5TDTsx9eovJk8/UrDBhziTyShJ0WneHCIgTGtIyFs1hMSDYwAs7xrJCe9tjCJc
PdW35lVY8Ky29R8Inhg0PgWMRxtnOC9NeXcI1c37gUh473TamZqUrHqjnZT2IPym
VtRmorkCgYEAucjWGqTIZG5qa2VoK38KnstopJbMwC3QNWSmJP9nxy+g3Xdy2AzI
3C1JqXVS7pm5NaZhszY33CSjiihk9ktYF5psuB8u1g42ChCQc+CifRdIS5h3UCG5
UYCDSarZv1a4DvD8EsG4O1KMgLDbYoC4XOFNLFDziaOkCU4IPjsAA5A=
-----END RSA PRIVATE KEY-----

- Arbitrary name
ec2-18-231-189-79.sa-east-1.compute.amazonaws.com

- Hostname – can be private IP address of your NFS server
172.31.38.44

- Username – ec2-user (since NFS server is based on EC2 with RHEL 8)
    
- Remote directory – /mnt/apps since our Web Servers use it as a mointing point to retrieve files from the NFS server

-Ubuntu do-release-upgrade
`sudo do-release-upgrade`
![ubuntu upgrade](do-release-upgrade.png)

-Ubuntu do-release-upgrade
![ubuntu upgrade](upgraded.png)