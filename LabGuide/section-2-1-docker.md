**Table of Contents**

- [Docker](#)
	- [Docker Toolbox](#)
		- [Exercise-1](#)
			- [Installing Docker-Toolbox](#)
			- [Tune your VM for faster performance](#)
		- [Exercise-2](#)
			- [Working with Docker Machine Utility](#)
		- [Exercise-3](#)
			- [Ssh into your Docker VM](#)
		- [Exercise-4](#)
			- [Creating source file.](#)
		- [Exercise-5](#)
			- [Setting the Environment variables using source file](#)
	- [Nxtoolkit Container](#)
		- [Exercise-6](#)
			- [Creating nxtoolkit image](#)
		- [Exercise-7](#)
			- [Spin up a nxtoolkit Container using this image](#)
	- [Ansible Docker Container](#)
		- [Exercise-8](#)
			- [Build Ansible Dockerfile](#)
		- [Exercise-9](#)
			- [Creating Ansible Container using Docker](#)
			- [Configuring the container](#)
- [Creating Docker host for Tools](#)
	- [Jenkins  Container](#)
		- [Exercise 9](#)
			- [Creating Jenkins Container](#)
		- [Create Dockerfile](#)
			- [Pull the Jenkins image from dockerhub](#)
			- [Build the new image](#)
	- [Gitlab Container](#)
		- [Create gitlab docker compose file](#)
		- [Spin up gitlab container](#)
		- [Configure gitlab](#)
- [Docker Tips](#)

#Docker
Docker is container management software for linux.  You can create lightweight containers (isolated environment) to run your application.  We will be using Dockers are lot in this lab. We will be using to install all the devops tools that we need using dockers.  

To do off box programming , you will need to install python, ansible etc on your laptop.  To get you started quickly and to save time, we will use docker to quickly download a pre-installed and pre-configure containers.  Containers are becoming very popular now, so this will give you some hands on  docker  and how to use it.  Here is a quick comparison between container and hypervisors
![docker](/images/docker-i-1.png)

Learn more about dockers at https://www.docker.com/


```
Cisco VPN/anyconnect breaks docker communication.  
Make sure you disconnect from anyconnect and use blizzard or wired connection while doing docker exercises.
```
##Docker Toolbox
Docker-toolbox allows you to run docker on your laptop (both in mac or windows).  Follow the link below to install it on your laptop.  Basically it downloads a virtual box boot2docker VM and runs docker engine inside it.  By default this VM is named `default`.  Docker has created `docker-machine` utility to manage docker VM from command line. Basically you can `start / stop / restart` the Docker VM using docker-machine utility.  You can create additional docker machine using the `create` command.  Learn more about it here https://docs.docker.com/machine/

![docker](/images/docker-i-2.png)

###Exercise-1
####Installing Docker-Toolbox
1. Download and install docker toolbox. Use all the default settings for the installation.
	https://www.docker.com/products/docker-toolbox


####Tune your VM for faster performance
1. Open up virtual box
2. shutdown the `default VM`.  Right click and select power off.
2. select the `default` VM
3. click on `settings`
4. click on `system` then `Motherboard` , them `processor`
4. increase the memory to `4G and CPU to 4`. (higher the better)
5. do not start the VM yet. we will do it in the next exercise.

###Exercise-2
####Working with Docker Machine Utility
When you installed docker tool, `docker-machine` automatically got installed. `docker-machine` utility lets you manage docker engines on different hypervisors including virtualbox. Its like a manager of docker engine. Docker engines can be hosted locally on your virtualbox or remotely on the cloud.

***Make sure you do not have any VPN connections such as Cisco's Anyconnect turned on. This will break the docker functionality***

1. Open up Terminal window (windows use open up `gitbash`)
2. Under home directory create a folder and name it `training`
3. cd `~`
4. `mkdir training`
4. `docker-machine ls`
5. `docker-machine start default`
6. `docker-machine ls`
![docker1](/images/docker-m-1.png)
5. This should show you all the docker engines that this docker machine is managing.  Your installation of docker toolbox should have created a docker VM named `default` in the Virtualbox.  This a ubuntu VM  that runs the docker engine.  You can use `docker-machine` to manage this docker engine.  Here are few commands you can run.

### Exercise-4
####Creating source file.
Source file are used to set the environment variable to communicate to the docker process running install a VM installed in the virtualbox.

1. In the same terminal window
2. type `docker-machine env default`
3. Copy the content to the clipboard.
4. Open the **Atom** Editor.
5. Go to `File` menu and then select `Add Project Folder`

	![atom100](/images/atom-100.png)

6. Navigate to the `training` folder in your home directory.
7. Click `Open` to open this folder in the `ATOM` editor
6. Right click on `training` folder and select `New File`. Name it `source.docker`.
7. Paste the clipboard content to this file.
8. Save it   `CMD+S or Ctrl+S`

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

2. This will show you all the containers running on your docker host.  Your docker host name is `default`.  You should not see any containers at this time.  We going to create containers in the next exercise.


### Common Docker-machine commands
```
docker-machine status default
docker-machine ip default
docker-machine stop default
docker-machine start default
```


##Extras
**Do not this in the class**

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
