#gitlab

##Creating ansible repository on gitlab.
###Exercise-1
#### Configure gitlab

1. Open up chrome browser and point it to:
2. `http://<ip of the docker node>`
3. create a new user

  ![gitlab](/images/gitlab-100.png)

	```
	<yourid>/cisco123

	```

4. Click on create `New project` and name it `ansible`

  ![gitlab](/images/gitlab-300.png)

5. Click on `Create Project`.
6. Use the information on this page for the next exercise.

###Exercise-2
#### Cloning the Repository
1. Switch to the terminal window
2. `cd training`
3. Follow the instructions from the gitlab site to create a new repository.

  ![gitlab](/images/gitlab20.png)
4. Do not copy and paste everything at once.  Do line by line.
5. Modify the first line as follows. Add your username and password to the url ..hemakuma:cisco123@

  `git clone http://hemakuma:cisco123@192.168.99.102/hemakuma/ansible.git `

  ![gitlab](/images/gitlab-301.png)

###Exercise-3
#### Verify that file got uploaded to gitlab
1. Switch back to you browser
2. Refresh it
3. Click on the `file tab`

  ![gitlab](/images/gitlab-103.png)

4. Make sure you see "README.md".

  ![gitlab](/images/gitlab-303.png)

###Exercise-4
#### Scripting the git push
For the lab environment, we want to quickly push the changes to the remote repo on the gitlab. In this exercise we going to create a script to automate this process.

1. Switch to `ATOM` Editor
2. Go to the `ansible` folder.
3. Right click and select `New File`
4. name it `gitpush.sh`
5. add the following content to the file

    ```
    #!/bin/bash
    echo 'updating git'
    git add .
    git commit -m 'updated'
    git push
    ```
6. Save the file `cmd + S`

###Exercise-5
#### Modifying the README file.
1. Open up the `README.md` file from the `ansible` folder using `ATOM`
2. Add some contents to it eg `this is my first gitlab edit`
3. Save it.

###Exercise-6
#### Push the Updated file to gitlab using script
1. Switch back to the terminal window.
2. you should be in the `ansible` directory  (`training/ansible`)
3. make the gitpush.sh script executable.
4. `chmod +x gitpush.sh`
5. Now use it to push the `README.md` to gitlab.
6. type
7. `./gitpush.sh`

###Exercise-7
#### Verify the file has changed on gitlab.
1. Switch to browser
2. Take a look at the README.md
3. you should see the new contents.

  ![gitlab](/images/gitlab-302.png)

4. You can compare what changed by clicking on the `README.md` and then on the `History` tab. Select the `update` you want to compare.

  ![gitlab](/images/gitlab-304.png)


##Creating Containers repository on gitlab.

###Exercise-1
#### Create containers Repository
1. Switch to gitlab page.
2. Click on + sign to create a new project

  ![gitlab](/images/gitlab-500.png)
3. name it `containers`
4. click `create project`

###Exercise-2
#### Put the `containers` folder under git control
1. switch to terminal Window
2. cd `training`
3. cd `containers`
4. type the following.  Remember, this time we are not cloning it. We are just putting a existing folder under git control.

  ![gitlab](/images/gitlab-501.png)

##Creating nxtoolkit repository on gitlab.

###Exercise-1
#### Create nxtoolkit project
1. login to the gitlab using your own account.
2. create a new `project`, name it `nxtoolkit`

    ![gitlab](/images/nxtoolkit-20.png)

###Exercise-2
#### Cloning the Repository
1. Switch to the terminal window
2. `cd training`
3. Follow the instructions from the gitlab site to create a new repository by cloning it.

  ![gitlab](/images/gitlab20.png)
4. Do not copy and paste everything at once.  Do line by line.
5. Modify the first line as follows. Add your username and password to the url ..hemakuma:cisco123@

  `git clone http://hemakuma:cisco123@192.168.99.102/hemakuma/nxtoolkit.git `

  ![gitlab](/images/gitlab-301.png)



### Exercise 1
### Create another gitlab account
You will use the 2nd account to review the changes.  You will pretend to be the reviewer.

1. sign out from gitlab.
2. Fill in the form for `New User` account

![gitlab](/images/gitlab-400.png)

3. Sign back in gitlab using the new account.  Make sure it works.


create a project using your own accounts

give access to the project all the users ...developer1, reviewer1

make sure to give them "Developer"  access


on you laptop create a directory called gitlab

create another folder called developer1

clone the repo as a develper

create a branch

make changes and pushd

request for merge and assign to another user

here is where go review will come in handy

login in as another user ...review the changes and merge it


H

```
git branch mtuchange
git checkout mtuchange

or
git checkout -b mtuchange

```

###Steps:

Clone project
git clone git@example.com:project-name.git
Create branch with your feature
git checkout -b $feature_name
Write code. Commit changes
git commit -am "My feature is ready"
Push your branch to GitLab
git push origin $feature_name
Review your code on commits page
Create a merge request
Your team lead will review the code & merge it to the main branch
