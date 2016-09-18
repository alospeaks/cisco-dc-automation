
#Docker Containers
Lets build some containers on our docker engine.


## Gitlab Container
We need gitlab for our source control.  We could use github but we wanted to see fully integration with Jenkins and other tools, therefore installed it locally.  With dockers, this is pretty issue.  If you don't understand something, please google it up. We will be using docker-compose and dockerfiles. Please google it up and read little bit on it.

###Exercise-1
####Create gitlab docker compose file
Docker compose is one way of spinning up docker containers.

1. Switch to the terminal window
  2. type `docker-machine ip default`
  3. Copy the ip address from the output. you will need to modify the docker compose file in step 2.
2. Switch to `ATOM` editor
  1. Right click on the `Training` Folder and select new folder. Name it `gitlab`
  2. Right click on the `gitlab` folder and select `New File`
  3. name it `docker-compose.yml`
  4. Paste the following content into the file and save it:
  5. Make sure to change the `external_url` to reflect the ip address of you docker machine.

      ```
      gitlab:
        image: 'gitlab/gitlab-ce:latest'
        container_name: gitlab
        restart: always
        hostname: 'gitlab.cisco.com'
        environment:
          GITLAB_OMNIBUS_CONFIG: |
            external_url 'http://192.168.99.102'
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

1. Switch to the terminal window
2. cd `gitlab`
3. type ` docker-compose up -d`
4. this should start up the gitlab container.  It will take couple of mins to download the images.

  ![docker-comp](/images/docker-comp-1.png)

5. Wait for this process to finish.  It should take couple of mins.
6. Verify that your gitlab container is running. Type  `docker ps`
7. type `docker-machine ip default`  Note down this ip somewhere, you will need it throughout this lab.
10. Wait for few mins for the services to come up inside the container then do next exercise.

###Exercise-3
#### Configure gitlab

1. Open up chrome browser and point it to:
2. `http://<ip of the docker node>`
3. create a new user

  ![gitlab](/images/gitlab-100.png)

	```
	<yourid>/cisco123

	```

4. Click on create `New project` and name it `traininglabs`

  ![gitlab](/images/gitlab-101.png)

#### Cloning the Repository
1. Switch to the terminal window
2. `cd training`
3. Follow the instructions from the gitlab site to create a new repository.
4. Do not copy and paste everything at once.  Do line by line.
5. Modify the first line as follows. Add your username and password to the url ..hemakuma:cisco123@

  git clone http://hemakuma:cisco123@192.168.99.102/hemakuma/traininglabs.git  

  ![gitlab](/images/gitlab-102.png)

### Verify that file got uploaded to gitlab
1. Switch back to you browser
2. refesh it
3. click on the file tab

  ![gitlab](/images/gitlab-103.png)

4. Make sure you see "README.md".

  ![gitlab](/images/gitlab-104.png)


## Jenkins  Container
### Exercise 4
####Creating Jenkins Container
1. Switch to `Atom` Editor
2. create a new folder under `training`.
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
USER root
RUN apt-get update && apt-get -y upgrade && apt-get install -y  vim git python curl openssh-server  python-pip python-dev build-essential libssl-dev libffi-dev
RUN pip install --upgrade pip
RUN pip install ansible==2.1.1
RUN pip install markupsafe
RUN pip install cryptography
##if this fails, use pip install cryptography --upgrade; failed in jenkins container
## upgrade to the latest VERSION
RUN pip install ansible --upgrade
#RUN echo 172.16.123.135 leaf1 > /etc/hosts

```
7. `Cmd + S`, to save the file

####Build the new image
1. Open up a terminal window,
2. navigate to the training directory
3. type `source source.docker`
4. cd to `jenkins` folder
5. type `Docker build -t  jenkins-ansible --tag hemakuma/jenkins-ansible .`
6. This should build a new image
7. Verify that the images is in your local repo: `docker images | grep jenkins`
8. Run the jenkins-ansbile container based on this image.

	`docker run --name jenkins -h jenkins -it --restart=always -p 8080:8080 -p 50000:50000 hemakuma/jenkins-ansible`

14. Wait until u see message `Jenkins is fully up and running`

  ![jenkins](/images/jenkins-11.png)

**We will configure Jenkins Server in Jenkins section later**

## Nxtoolkit Container
Nxtoolkit container is prebuilt container that has Cisco nxtoolkit installed.  nxtoolkit provides python libraries and examples on how to interact with Cisco's Nexus switches using NXAPI.
### Exercise-5
####Creating nxtoolkit image
1. Go to your `ATOM` Editor
2. Right click on the `training` folder and select `New Folder`. Name it `nxtoolkit`

  ![docker](/images/docker-c-2.png)

3. Now Right click on the `nxtoolkit` folder and select `New File`
4. Name this file `Dockerfile`
4. Copy and paste the content from here to this new file.  
https://github.com/Hemakuma/training/blob/master/nxtoolkit/Dockerfile  
This docker file will create a docker image for nxtoolkit
5. Save the file `CMD-S`. Name the file `Dockerfile`.  Click save. (for windows users use ctrl+S)

####Creating the docker images for nxtoolkit
6. Switch back to your terminal window (Windows users use Git Bash)
go to training directory
6. `cd nxtoolkit`
7. `ls` <-- make sure your dockerfile is in this directory.
8. type `docker build -t nxtoolkit --tag hemakuma/nxtoolkit . `
9. or u can tag it too, make sure to use your id ..

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
1. Switch to `ATOM` editor
2. Right click on the `Training` folder and select `New Folder`.  Name it `ansible`.
3. Now Right click on the `ansible` folder and select `New File`,  and name this file as `Dockerfile`
4. Copy and paste the content from here to this new file.  
https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/Dockerfile_ansible  
5. Build the image. From the terminal, type  
    `docker build -t ansible --tag hemakuma/ansible .`
6. Verify that ansible image is created.  
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



##Using Docker Compose



**STOP HERE , we will do the rest in the Jenkins Section**

#Docker Tips
If your boot2docker can not resolve dns entry?
`docker-machine default ssh`

```
echo "nameserver 8.8.8.8" > /etc/resolv.conf && sudo /etc/init.d/docker restart

```

Sometimes the images gets large and u do not have enough space on the VM
` df -h `

to check the disk usage.

you can use --disksize option of docker-machine create to create a bigger size disk.  By default its only 2G.

**Some common docker commands**
```

docker ps
docker run
docker attach <container name>
docker info
docker inspect <container name>

```
