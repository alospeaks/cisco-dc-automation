#Jenkins
Jenkins is an open source tool that is used for scheduling and running automated tests and deployment.  It is essentially the friendly butler of automation who will handle all of your automation tasks for you once you tell him to do so and he only needs to be told once.

In the Server world, Jenkins can be used to perform the typical build server work, such as doing continuous/official/nightly builds, run tests, or perform some repetitive batch tasks.

### Exercise-1
####Update the hosts file
1. Open another terminal window and navigate to training folder
2. source `source source.docker`
3. Find out the ip address of your `nxosv` switch.  you can login into the switch and type `show ip int brief vrf  management`.  Note down the ip.
4. Update the hosts files on the jenkins server  since we are not using dns
5. type

	`docker exec -it jenkins /bin/sh -c "echo 172.16.123.135 leaf1 > /etc/hosts"`

6. repeat this for all the switches you have.

### Exercise-2
####Getting Jenkins login password
1. Get the admin password
	3. type `docker exec -it jenkins  cat /var/jenkins_home/secrets/initialAdminPassword `
	4. copy the password
	5. type `docker-machine ip default` and note down the ip address of the docker host

### Exercise-3
####Login into Jenkins Server
6. Open up a chrome browser and login in.
7. `http://<ip>:8080`
8. type in the admin password that you copied previously.

	![jenkins](/images/jenkins-12.png)
10. Click `Select plugins to install`. and select none (we will install plugins manually)

	![jenkins](/images/jenkins-200.png)

11. Select none (we will install plugins manually)

	![jenkins](/images/jenkins-13.png)

12. Create a user account (<yourid>/cisco123)

	![jenkins](/images/jenkins-201.png)

13. Click next and then `start using jenkins`


### Exercise-4
#### Installing jenkins plugins

We need gitlab plugin for now.  Lets install them.

1. Go to the dashboard
2. on the left side, click on `Manage Jenkins`
3. On the right side, click on `Manage Plugins`

	![jenkins](/images/jenkins-15.png)
3. Then select plugins
	1. Click on the `Available` plugins
	2. on the `Filter` , type git
	3. Find the following plugin and check the select box.
		1. gitlab plugin

	![jenkins](/images/jenkins-16.png)

5. Click on `Install without restart`

	![jenkins](/images/jenkins-17.png)

6. Once install, you need to restart your server. Check the `restart box` at the end of the screen or
7. On the url of the browser type `http://<ip>:8080/restart`

### Exercise-5
#### Creating a job
Jenkins Job Builder takes simple descriptions of Jenkins jobs in YAML or JSON format and uses them to configure Jenkins. You can keep your job descriptions in human readable text format in a version control system to make changes and auditing easier

1. Click on `Create new Job`

 	![jenkins](/images/jenkins-jobs-1.png)
3. Name the job `deploy-prod`
4. use `free style project`

	Freestyle project: This provides the ability to create a completely custom job that can behave in any way we choose.

	![jenkins](/images/jenkins-jobs-2.png)

5. Switch to your gitlab tab on the browser.  
	1. Click on the `Project`
	2. copy the url for the `ansible` project.

	![jenkins](/images/gitlab-305.png)

5. Fill in the "Source Code Management Form"

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

8. Click on `Build Now`

 ![jenkins](/images/jenkins-jobs-10.png)

9. Take a look at the console output

 ![jenkins](/images/jenkins-jobs-9.png)


### Exercise-6
#### Using console to see the log output


### Exercise-6
#### Manually running a job
you can only run it manually now since your jenkin server is not reachable via internet.




###Troubleshooting
**Tips**

Sometime you will need to login into jenkins container to troubleshoot this. Use this method:

`docker exec -u 0 -it jenkins bash`

or

`docker exec -it jenkins /bin/bash`

All the files are under ``/var/jenkins_home/workspace/``  ; your git will be cloned into this directory
