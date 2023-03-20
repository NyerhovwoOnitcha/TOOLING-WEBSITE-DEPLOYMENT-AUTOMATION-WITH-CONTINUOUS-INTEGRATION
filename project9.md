### Install Jenkins
```
Install JDK

sudo apt update

sudo apt install default-jdk-headless

Install Jenkins

wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'

sudo apt update

sudo apt-get install jenkins
```
#### Ensure Jenkins is running 
`sudo systemctl status jenkins`

### Open TCP port 8080 as that is the jenkins default port

### Log into jenkins
![](./images/Annotation%202023-02-08%20221156.jpg)

###  Configure Jenkins to retrieve source codes from GitHub using Webhooks

### Enable webhook in yout github repo
![](./images/addd%20webhook.jpg)

![](./images/webhook%20successful.jpg)

### Go to your jenins console and select new item

![](./images/new%20job.jpg)

### Enter a new item/job name and choose freestyle project

![](./images/freestyle%20project.jpg)

### In your job configuration, choose git repository and provide the link to your tooling website repo and add credentials

![](./images/git%20repo%20link.jpg)

![](./images/added%20git%20repo%20in%20jenkins.jpg)

### Continure your configuration, in the "Build Triggers" section select Github hook trigger for GITScm polling

![](./images/configure%20to%20trigger.jpg)

### Proceed to the "Post Build Actions" section and here select the "archive all files" option.

![](./images/post%20build%20action.jpg)

### Save your configuration. Go ahead and make little changes to the README.md file of your tooling website repo and push to the masters branch and your build shold be automatically triggered.

![](./images/modified%20readme%20file.jpg)

![](./images/pushed%20edit.jpg)

### Check your jenkins console for automatic builds and for artifacts

![](./images/automatic%20build.jpg)

![](./images/artifacts.jpg)

### check to see the artifacts stored locally
![](./images/artiacts%20stored%20locally.jpg)

### Configure jenkins to copy files to NFS server via SSH
### First install the publish over ssh plugin

![](./images/install%20PoSSH%20plugin.jpg)

### Configure the plugin, on your dashboard select "manage plugins" -> "configure system" -> scroll down to the "publish over ssh" section and configure it to connect to the NFS server

- Provide a private key (content of .pem file that you use to connect to NFS server via SSH/Putty)
- Arbitrary name
- Hostname – can be private IP address of your NFS server
- Username – ec2-user (since NFS server is based on EC2 with RHEL 8)
- Remote directory – /mnt/apps since our Web Servers use it as a mounting point to retrieve files from the NFS server 

![](./images/system%20config%20for%20ssh%20plugin.jpg)


### Test the configuration to make sure it returns as "success"

![](./images/success.jpg)

### Return to your last job and edit the configuration, go to "add post build acton section" and add "publish over ssh". Select the "send build artifacts over ssh" option

![](./images/send%20build%20artifacts%20over%20ssh.jpg)

### Configure it to send all files probuced by the build into our previouslys defined remote directory

![](./images/send%20all%20files.jpg)

### Check the remote directory before the build and transfer is triiggered

![](./images/before%20publish%20over%20ssh.jpg)

### Set permissions on the remote dierectory where the artifacts will be sent

`sudo chmod -R 777 /mnt`

`sudo chown -R nobody:nobody /mnt`

![](./images/change%20permission%20and%20ownership.jpg)

### Trigger the build
![](./images/build%20successful.jpg)

![](./images/build%20succ.jpg)

![](./images/build%20suc.jpg)