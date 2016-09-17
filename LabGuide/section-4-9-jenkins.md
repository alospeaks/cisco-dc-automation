#Jenkins


###Getting Jenkins login password
7. Get the admin password
	1. Open another terminal window and navigate to training folder
	2. source `source source.docker`
	3. type `docker exec -it jenkins  cat /var/jenkins_home/secrets/initialAdminPassword `
	4. copy the password
	5. type `docker-machine ip default` and note down the ip address of the docker host

###Login into Jenkins Server
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
Installing jenkins plugins
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
6. Once install, you need to restart your server. Check the 'restart box' at the end of the screen or
7. On the url of the browser type `http://<ip>:8080/restart`

### Creating a job
1. Click on "Create new Job"

 	![jenkins](/images/jenkins-jobs-1.png)
3. Name the job `deploy-prod`
4. use `free style project`

	![jenkins](/images/jenkins-jobs-2.png)

5. Login into your github account and get the url for the `training` repository.

![jenkins](/images/jenkins-jobs-3.png)

Fill in the "Source Code Management Form"

![jenkins](/images/jenkins-jobs-4.png)

![jenkins](/images/jenkins-jobs-8.png)

6. set the trigger
7.
![jenkins](/images/jenkins-jobs-6.png)

```
#!/bin/bash
echo "Running Ansible against: $FQDN"
# http://www.ansibleworks.com/docs/gettingstarted.html#a-note-about-host-key-checking
export ANSIBLE_HOST_KEY_CHECKING=False
pushd /var/jenkins_home/workspace/deploy-prod/ansible/
    ansible-playbook -i hosts <playbook>.yml
popd

```

### Manually running a job


### Using console to see the log output
