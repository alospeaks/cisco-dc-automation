
#Docker Containers
Lets build some containers on our docker engine.


## Gitlab Container
We need gitlab for our source control.  We could use github but we wanted to see fully integration with Jenkins and other tools, therefore installed it locally.  With dockers, this is pretty issue.  If you don't understand something, please google it up. We will be using docker-compose and dockerfiles. Please google it up and read little bit on it.

###Exercise-1
####Create gitlab docker compose file
Docker compose is one way of spinning up docker containers.

1. Switch to the terminal window
  2. type `docker-machine ip default`
  3. Copy the ip address from the output
2. Switch to `ATOM` editor
  1. Right click on the `Training` Folder and select new folder. Name it `gitlab`
  2. Right click on the `gitlab` folder and select `New File`
  3. name it `docker-compose.yml`
  4. Paste the following content into the file and save it:
  5. Make sure to change the `external_url` to reflect the ip address of you docker machine.

      ```
      gitlab:
        image: 'gitlab/gitlab-ce:latest'
        restart: always
        hostname: 'gitlab.cisco.com'
        environment:
          GITLAB_OMNIBUS_CONFIG: |
            external_url 'http://192.168.99.101'
            # Add any other gitlab.rb configuration here, each on its own line
        ports:
          - '80:80'
          - '443:443'
          - '2222:22'

        volumes:
          - '/srv/gitlab/config:/etc/gitlab'
          - '/srv/gitlab/logs:/var/log/gitlab'
          - '/srv/gitlab/data:/var/opt/gitlab'

      ```



###Exercise-2
####Spin up gitlab container

1. On the terminal window
2. go to training folder.  ` cd training`
4. set the environment variable. type
5. `source source.docker`
6. cd `gitlab`
7. type ` docker-compose up`
8. this should start up the gitlab container.
9. Verify `docker ps`
10. Wait for few mins

###Exercise-3
#### Configure gitlab

1. Open up chrome browser
2. http://<ip of the docker node>
3. create a new user

	```
	admin/cisco123

	```

4. create new project and name it `training`

![gitlab](/images/gitlab-1.png)



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


#Creating Docker host for Tools
We will create a different docker host jenkins, gitlab and gerrit


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

**STOP HERE , we will do the rest in the Jenkins Section**

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
