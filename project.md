# Continuous Integration Pipeline with Jenkins Server
1. Install and Configure Jenkins
**Create instance in south africa region**
![Ubuntu 20. LTS Instance](Images/jenkins.PNG)
`ssh -i "jen-key.pem" ubuntu@ec2-15-228-54-237.sa-east-1.compute.amazonaws.com`
2. Install JDK
`sudo apt update`
![apt updated](Images/apt-update.PNG)
`sudo apt install default-jdk-headless`
![jdk default](Images/jdk.PNG)
3.Install Jenkins

`wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -`
`sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'`
`sudo apt update`
`sudo apt-get install jenkins`
![jenkins installed](Images/jenkins-install.PNG)

- Make sure Jenkins is up and running
`sudo systemctl status jenkins`
![jenkins status](Images/stat.PNG)

5. By default Jenkins server uses TCP port 8080 – open it by creating a new Inbound Rule in your EC2 Security Group
![Inbound rule](Images/port.PNG)

- Perform initial Jenkins setup.

From your browser access http://15.228.54.237:8080

You will be prompted to provide a default admin password 
- Run
`sudo journalctl -u jenkins.service`
or
`sudo vi /var/lib/jenkins/secrets/initialAdminPassword`
 d84735f4243e4faf8ea684558518804e
![jenkins logged in](Images/site.PNG)
-Setup Username admin and password as above
![jenkins user setup](Images/setup.PNG)

## Step 2 – Configure Jenkins to retrieve source codes from GitHub using Webhooks
1. Enable webhooks in your GitHub repository settings
[add webhooks](https://github.com/J-Raji/Project9/settings/hooks/397986019)

2. Go to Jenkins web console, click "New Item" and create a "Freestyle project"
-Install Git Server update
![tooling github linked](Images/git-link.PNG)

3. Click "Configure" your job/project and add these two configurations
Configure triggering the job from GitHub webhook:


- Archive the artifact on Post Actions
![saved and applied](Images/post.PNG)

-confirm build
![confirm update of project in Jenkins](Images/build.PNG)

## Configure Jenkins to copy files to NFS server via SSH
1. Install "Publish Over SSH" plugin.
![Installed Publish over ssh](Images/publish.PNG)
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

![Ubuntu upgrade](Images/do-release-upgrade.PNG)

-Upgrade complete

![completed](Images/upgraded.PNG)


Jenkins API Token: 112be6335f77896d074cb7090fae6e05ea

Github ID: bccaf954-498d-4841-98f3-c8e51b81452d
Client Key: 2d618aa183d799f3775a0193309841fc83b128c8
![Git APi created](Images/GitApi.PNG)

![app setting for tooling-git](Images/tooling-git.PNG)

-Blocker found
![error 403 created](Images/blocker.PNG)
-Resolved
1. Launch NFS instance on Rhel 8.
`ssh -i "nfskey.pem" ec2-user@ec2-52-67-48-33.sa-east-1.compute.amazonaws.com`
![Rhel 8](Images/rhel-nfs.PNG)
`lsblk`
![lsblk run](Images/list.PNG)
`df -h`
![df -h run](Images/ldf-h1.PNG)

`sudo gdisk /dev/xvdf`
`sudo gdisk /dev/xvdg`
`sudo gdisk /dev/xvdh`

-type p -next n with 8e00 -w -y
`lsblk`
![lsblk run](Images/newpart.PNG)

Install lvm2 
`sudo yum install lvm2`
![llvm2 run](Images/lvm2-new.PNG)
`sudo lvmdiskscan`
![lvmdiskscan run](Images/lvmscan.PNG)

Make  physical volumes
`sudo pvcreate /dev/xvdf1` 
`sudo pvcreate /dev/xvdg1` 
`sudo pvcreate /dev/xvdh1`
`sudo pvs`
![pvs run](Images/pvs2.PNG)

Volume Groups

`sudo vgcreate nfsdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1`

Verify 

`sudo vgs`

Create
`sudo lvcreate -n lv-opt -L 14G nfsdata-vg`

`sudo lvcreate -n lv-apps -L 14G nfsdata-vg`

`sudo lvcreate -n lv-logs -L 14G nfsdata-vg`

Confirm Logical volumes 

`sudo lvs`

![lvs run](Images/lvs2.PNG)

Verify entire setup 
`sudo vgdisplay -v` #view complete setup - VG, PV, and LV

![see all](Images/vgdisplay.PNG)

![see all](Images/vgdisplay1.PNG)

![see all](Images/vgdisplay2.PNG)

Run `lsblk`
![lsblk run](Images/lsblk3.PNG)

Format the LV with xfs filesystem 

`sudo mkfs -t xfs /dev/nfsdata-vg/lv-opt`

`sudo mkfs -t xfs /dev/nfsdata-vg/lv-apps`

`sudo mkfs -t xfs /dev/nfsdata-vg/lv-logs`

Rename vg nfsdata-vg to webdata-vg 

`sudo vgrename nfsdata-vg webdata-vg`

Make directory

`sudo mkdir /mnt && cd /mnt`

Create /mnt/apps to be used by webservers 

`sudo mkdir -p /mnt/apps` 

Create /mnt/logs to be used by webserver logs 

`sudo mkdir -p /mnt/logs`

Create /mnt/opt to be used by Jenkins server 

`sudo mkdir -p /mnt/opt`

Mount lv-app on /mnt/apps – To be used by webservers 

`sudo mount /dev/webdata-vg/lv-apps /mnt/apps` 

Mount lv-logs on /mnt/logs – To be used by webserver logs

`sudo mount /dev/webdata-vg/lv-logs /mnt/logs` 

Mount lv-opt on /mnt/opt – To be used by Jenkins server in Project 8

`sudo mount /dev/webdata-vg/lv-logs /mnt/opt` 


Install NFS server, configure it to start on reboot and make sure it is u and running

`sudo yum -y update`

`sudo yum install nfs-utils -y`

![nfs-utils](Images/yum-ut.PNG)

`sudo systemctl start nfs-server.service` 

`sudo systemctl enable nfs-server.service`

`sudo systemctl status nfs-server.service`

![nfs- service](Images/nfs-systemctl.PNG)

Confirm Subnet link to cidr 

![cidr](Images/subnet-link.PNG)

Make sure we set up permission that will allow our Web servers to read, write and execute files on NFS:

`sudo chown -R nobody: /mnt/apps `

`sudo chown -R nobody: /mnt/logs` 

`sudo chown -R nobody: /mnt/opt`


`sudo chmod -R 777 /mnt/apps` 

`sudo chmod -R 777 /mnt/logs`

`sudo chmod -R 777 /mnt/opt`

`sudo systemctl restart nfs-server.service`

Sync 

`sudo rsync /mnt/logs` 

`sudo rsync /mnt/apps` 

`sudo rsync /mnt/opt`

Configure access to NFS for clients within the same subnet (example of Subnet CIDR – 172.31.0.0/20 ):

`sudo vi /etc/exports`

/mnt/apps <Subnet-172.31.0.0/20>(rw,sync,no_all_squash,no_root_squash) 

/mnt/logs <Subnet-172.31.0.0/20>(rw,sync,no_all_squash,no_root_squash) 

/mnt/opt <Subnet-172.31.0.0/20>(rw,sync,no_all_squash,no_root_squash)

Esc + :wq!

`sudo exportfs -arv`

Check which port is used by NFS and open it using Security Groups (add new Inbound Rule)

`rpcinfo -p | grep nfs`

![rpcinfo](Images/rpcinfo.PNG)

[Jenkins](Images/http://18.230.184.147:8080)

Modification the job/project to copy artifacts over to NFS server.

On main dashboard select "Manage Jenkins" and choose "Configure System" menu item.

Scroll down to Publish over SSH plugin configuration section and configure it to be able to connect to your NFS server:

    Provide a private key (content of .pem file that you use to connect to NFS server via SSH/Putty)
    nfskey content

    Arbitrary name
    NFS

    Hostname – can be private IP address of your NFS server
    172.31.12.167

    Username – ec2-user (since NFS server is based on EC2 with RHEL 8)
    Remote directory – /mnt/apps since our Web Servers use it as a mointing point to retrieve files from the NFS server

In Configure Global Security > CSRF Protection
Crumb Issuer(Default Crumb Issuer)
Enable proxy compactibility

![proxy set](Images/crumb-proxy.PNG)

Publish over ssh done

![test success](Images/success.PNG)

`sudo cat /mnt/apps/README.md`

![access README.md](Images/readme.PNG)
