**Table of Contents**
- [Docker](#)
	- [Docker Toolbox](#)
		- [Exercise-1](#exercise-1)
			- [Installing Docker-Toolbox](#)
		- [Exercise-2](#exercise-2)
			- [Working with Docker Machine Utility](#)
		- [Exercise-3](#exercise-3)
			- [Ssh into your Docker VM](#)
		- [Exercise-4](#exercise-4)
			- [Creating source file.](#)
		- [Exercise-5](#exercise-5)
			- [Setting the Environment variables using source file](#)
	- [Nxtoolkit Container](#)
		- [Exercise-6](#exercise-6)
			- [Creating nxtoolkit image](#)
		- [Exercise-7](#exercise-7)
			- [Spin up a nxtoolkit Container using this image](#)
	- [Ansible Docker Container](#)
		- [Exercise-8](#exercise-8)
			- [Build Ansible Dockerfile](#)
		- [Exercise-9](#exercise-9)
			- [Creating Ansible Container using Docker](#)
- [Docker Tips](#docker-tips)


#Docker

To do off box programming , you will need to install python, ansible etc on your laptop.  To get you started quickly and to save time, we will use docker to quickly download a pre-installed and pre-configure container.  Containers are becoming very popular now, so this will give you some hands on  docker  and how to use it.  This is the future, so do spend some time learning this technology.  Here is a quick comparison between container and hypervisors
![docker](/images/docker-i-1.png)

```
Cisco VPN/anyconnect breaks docker communication.  
Make sure you disconnect from anyconnect and use blizzard or wired connection while doing docker exercises.
```
##Docker Toolbox
Docker-toolbox allows you to run docker on your laptop (both in mac or windows).  Follow the link below to install it on your laptop.  Basically it downloads a virtual box Ubuntu VM and runs docker engine inside it.  By default this VM is named "default".  Note, Docker has created docker-machine utility to manage this docker VM from command line. Basically you can `start / stop / restart` the Docker VM using docker-machine utility.  You can create additional docker machine using the `create` command.

![docker](/images/docker-i-2.png)

###Exercise-1
####Installing Docker-Toolbox
1. Download and install docker toolbox. Use all the default settings for the installation.
	https://www.docker.com/products/docker-toolbox

###Exercise-2
####Working with Docker Machine Utility
When you installed docker tool, docker-machine automatically got installed. `docker-machine` utility lets you manage docker engines. Its like a manager of docker engine. Docker engines can be hosted locally on your virtualbox or remotely on the cloud.

***Make sure you do not have any VPN connections such as Cisco's Anyconnect turned on.***

1. Start `Docker Quickstart Terminal`.  (Mac Users: `cmd + space` then type `docker quickstart`)  (Windows `Users: Start > All`)
2. `Programs > Docker > Docker Quickstart Terminal)`
3. Run the following commands and examine the output.
4. `docker-machine ls`
![docker1](/images/docker-m-1.png)
5. This should show you all the docker engines that this docker machine is managing.  Your installation of docker toolbox should have created a docker VM named `default` in the Virtualbox.  This a ubuntu VM  that runs the docker engine.  You can use `docker-machine` to manage this docker engine.  Here are few commands you can run.

```
docker-machine status default
docker-machine ip default
docker-machine stop default
docker-machine start default
```


###Exercise-3
####Ssh into your Docker VM
You can also ssh into the docker VM. This is not needed for this lab, but just showing you the feature of docker-machine.
`docker-machine ssh  default`
Notice, after you run this command, you will get inside the Docker VM on the Virtual Box.  This VM runs your docker engine. You can run your docker command directly on this VM.  Go ahead and type
```
docker-machine ssh  default
docker ps
Exit
```

`exit` will get back to your Mac Terminal

### Exercise-4
####Creating source file.
Source file are used to set the environment variable to communicate to the docker process running install a VM installed in the virtualbox.

1. Open up a terminal (Windows users:  Restart `Git Bash` to make it aware of newly installed docker files)
2. type `docker-machine env default`
3. Copy the content to the clipboard.
4. Go to your **Atom** window
5. Create a new file under `training` folder
6. Right click on `training` folder and select `New File`
7. Paste the content of this file.
8. Save it as `source.docker`.  `CMD+S or Ctrl+S` to save

### Exercise-5
####Setting the Environment variables using source file
Now, you can use the `source.docker` file to source your environment variables.

1. Switch to your terminal Window and type the following commands.  
        ```
        cd ~
        cd  training
        source source.docker
        docker ps
        ```

2. This will show you all the containers running on your docker host.  Your docker host name default.  
You should not see any containers at this time.  We going to create containers in the next exercise.
To see your docker host, type `docker-machine ls`.


## Nxtoolkit Container
### Exercise-6
####Creating nxtoolkit image
1. Go to your `ATOM` Editor
2. Create a new folder called `nxtoolkit` in the `training` folder
![docker](/images/docker-c-2.png)
3. Now Right click on the `nxtoolkit` folder and select `New File`
4. Name this file `Dockerfile`
4. Copy and paste the content from here to this new file.  
https://github.com/Hemakuma/training/blob/master/nxtoolkit/Dockerfile  
This docker file will create a docker image for nxtoolkit
5. Save the file `CMD-S`. Name the file `Dockerfile`.  Click save. (for windows users use ctrl+S)
6. Switch back to your terminal window (Windows users use Git Bash)
go to training directory
6. `cd nxtoolkit`
7. `ls` <-- make sure your dockerfile is in this directory.
8. type `Docker build -t  nxtoolkit .`

    ***Don't forget the dot at the end. There is a space between nxtoolkit and dot.  For Windows users, don't forget the `winpty` in front of the docker command.
    It should build you the image for the nxtoolkit.  It should take about 5-10min to complete.***
9. Type `docker images` to verify it. Look for the nxtoolkit image.
![docker-3](/images/docker-c-3.png)
10. Now lets spin up one container using this image. This will be your nxtookkit container that have everything installed for you to get started with python coding.

###Exercise-7
####Spin up a nxtoolkit Container using this image
1. Switchback to the terminal window.
2. type `pwd` to verify your working directory. You should be in the `training/nxtoolkit` directory
3. Mac Users    
    `docker run --name nxtoolkit -h nxtoolkit -it --restart=always -v ~/training/nxtoolkit:/opt/nxtoolkit/myscripts  nxtoolkit`

4. Windows Users  
    Launch Docker Quickstart Terminal and run the following command:  
    `docker run --name nxtoolkit -h nxtoolkit -it --restart=always -v ~/training/nxtoolkit:/opt/nxtoolkit/myscripts  nxtoolkit`

    -v option basically lets you mount a volume inside your container so that you can access it from your laptop HDD.  This way you can edit your code on your laptop but it will be available inside the container.
    You should now be inside your nxtoolkit container.
    ![docker-4](/images/docker-c-4.png)
4. type `ls` <-- you should see your myscripts directory
5. `cd myscripts`  
    Note `myscripts` directory inside the container is mapped to your `/training/nxtoolkit` directory on your laptop.  This means, anything you create on your laptop in the `~/training/nxtoolkit` directory will be available inside your container and vice-versa.
6. Copy the samples into your working directory. Make sure u are in your `myscripts` directory inside the container
    1. `cp -r  /opt/nxtoolkit/samples/ .`
    2. Exit the container  by typing `exit` inside the container.
9. Verify that sample folder is available on your laptop
    1. Switch to  atom editor window
    2. You should see the samples folder under nxtoolkit folder.

    ![folder](/images/docker-c-5.png)

You are now ready to do off box programming.

##Ansible Docker Container
Lets install Ansible on  a docker container.

###Exercise-8
####Build Ansible Dockerfile
1. Switch to your terminal window.
2. Go to your `training` folder.
    1. `cd ~/training`
3. Type `source source.docker`
3. Create a directory to store ansible scripts
    1. `mkdir ansible`
    2. `cd ansible`
4. Create a Dockerfile.
5. Switch to `ATOM` editor
5. Right click on the `ansible` folder and select `New File`,  and name this file as `Dockerfile`
4. Copy and paste the content from here to this new file.  
https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/Dockerfile_ansible  
2. Build the image. From the terminal, type  
    `docker build -t ansible .`
3. Verify that ansible image is created.  
    1. `docker images`

###Exercise-9
####Creating Ansible Container using Docker

1. Spin up a nxos ansible container
    1. Mac Users  
    `docker run --name ansible -h ansible -it --restart=always -v ~/training/ansible:/nxos-ansible/myscripts --workdir /nxos-ansible/myscripts ansible`
    2. Windows Users  
    `winpty docker run --name ansible -h ansible -it --restart=always -v ~/training/ansible:/nxos-ansible/myscripts --workdir /nxos-ansible/myscripts ansible`

    This will start the container and log you in. From inside the container.

2. exit the container.  We will use this container later in the ansible exercises.
3. Go to your Atom editor and you should all these files under ansible folder.

####Configuring the container
configure the /etc/hosts and put the entry for your nxosv switch.

(can we do it via the dockerfile)

## Jenkins  Container
### Exercise 9
####Creating Jenkins Container
1. Switch to `Atom` Editor
2. create a new folder under training.
3. Right click on training and select `New Folder`
4. name it `jenkins`

###Create Dockerfile

Lets create a docker file to build a new image that has jenkins and ansible installed.  For jenkins, we will use prebuild image of Jenkins from dockerhub.  However we want to install ansible on it so that we can initiate deployment from the Jenkins server.

5. Right click on the `jenkins` folder and select `New File`
6. name it `Dockerfile`
7. paste the following

```
#VERSION 1.0
FROM jenkins
MAINTAINER Hemant Kumar, hemakuma@cisco.com
RUN apt-get update && apt-get -y upgrade && apt-get install -y  vim git python curl openssh-server  python-pip python-dev build-essential libssl-dev libffi-dev
RUN pip install --upgrade pip
RUN pip install ansible==2.1.1
RUN pip install markupsafe
RUN pip install cryptography
##if this fails, use pip install cryptography --upgrade; failed in jenkins container
## upgrade to the latest VERSION
RUN pip install ansible --upgrade
```
7. `Cmd + S`, to save the file

#### Pull the Jenkins image from dockerhub
1. Open up a terminal window,
2. navigate to the training directory
3.  type `source source.docker`
5. `cd to jenkins` folder
7. Lets pull the jenkins image from the dockerhub first
8. `docker images`
8. `docker pull jenkins`
9. `docker images`  <-- u should see the new image


####Build the new image
10. type `Docker build -t  jenkins-ansible .`
11. this should build a new image
12. `docker images | grep jenkins`
13. Run the jenkins-ansbile container based on this image.

	`docker run --name jenkins -h jenkins -it --restart=always -p 8080:8080 -p 50000:50000 jenkins-ansible`
14. Wait until u see message "Jenkins is fully up and running"

![jenkins](/images/jenkins-11.png)

7. Get the admin password
	1. Open another terminal window and navigate to training folder
	2. source `source source.docker`
	3. type `docker exec -it jenkins  cat /var/jenkins_home/secrets/initialAdminPassword `
	4. copy the password
	5. type `docker-machine ip default` and note down the ip address of the docker host
6. Open up a chrome browser and login in.
7. http://<ip>:8080
8. type in the admin password that you copied previously.

	![jenkins](/images/jenkins-12.png)
10. click install plugins and select none (we will install plugins manually)

	![jenkins](/images/jenkins-13.png)

12. create a user account (admin/cisco123)

	![jenkins](/images/jenkins-14.png)


**Tips**
To login into the jenkins container, try this
`docker exec -u 0 -it jenkins bash`

all the files are under /var/jenkins_home/workspace/  ; your git will be cloned into this directory

####
Installing jenins plugins
We only need github and gitlab plugin for now

1. Go to the dashboard
2. on the left side, click on "Manage Jenkins"

	![jenkins](/images/jenkins-15.png)
3. Then select plugins
	1. github plugin
	2. gitlab plugin

	![jenkins](/images/jenkins-16.png)
5. Don't  restart yet

	![jenkins](/images/jenkins-17.png)
6. Once install, you need to restart your server

![jenkins](/images/jenins-10.png)





### Creating a job


### Manually running a job


### Using console to see the log output


#Docker Tips
If your boot2docker can not resolve dns entry?
`docker-machine default ssh`

```
echo "nameserver 8.8.8.8" > /etc/resolv.conf && sudo /etc/init.d/docker restart

```

**Some common docker commands**
```

docker ps
docker run
docker attach <container name>
docker info
docker inspect <container name>

```
