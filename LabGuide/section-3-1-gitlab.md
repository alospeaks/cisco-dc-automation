#gitlab
## Introduction
GIT is a very popular and efficient open source Version Control System. It tracks content such as files and directories for changes. Files transition between 3 states, modified, staged and committed file.  Repository are local but if you want to share your codes with your team, you can also push it to remote/central repository.
![GitHub](/images/git-intro-1.png)

**Make sure you have git installed on your system.  See devops tools section.**



For this lab, we have installed our own gitlab server on the docker container. We will use this server for all our exercises.

##Creating `ansible` repository on gitlab.
###Exercise-1
#### Configure gitlab

1. Open up chrome browser and point it to:
2. `http://<ip of the docker node>`
3. change the admin password to `cisco123`

  ![gitlab](/images/gitlab-550.png)

4. create a new user

  ![gitlab](/images/gitlab-100.png)

	```
	<yourid>/cisco123

	```

4. Click on create `New project` and name it `ansible`

  ![gitlab](/images/gitlab-551.png)

  ![gitlab](/images/gitlab-300.png)

5. Click on `Create Project`.
6. Do not close this window. We will use the information on this page for the next exercise.

###Exercise-2
#### Putting `ansible` directory under git control.
1. Switch to the terminal window (docker quick start)
2. `cd training`
3. `cd ansible`
4. `touch READMe.md`
4. Follow the instructions from the gitlab site to create a new repository.

  ![gitlab](/images/gitlab20.png)
4. Do not copy and paste everything at once.  Do line by line.
5. For git commit command, use -m flag.  `git commit -m 'your message'`
5. Modify the first line as follows. Add your username and password to the url ..hemakuma:cisco123@

  `git remote add origin http://hemakuma:cisco123@192.168.99.100/hemakuma/ansible.git`


  ![gitlab](/images/gitlab-505.png)

###Exercise-3
#### Verify that file got uploaded to gitlab
1. Switch back to you browser
2. Refresh it
3. Click on the `file tab`

  ![gitlab](/images/gitlab-506.png)

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
6. Save and close the file `cmd + S`

###Exercise-5
#### Modifying the README file.
1. Open up the `README.md` file from the `ansible` folder using `ATOM`
2. Add some contents to it eg `this is my second gitlab edit`
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
Where is the History tab? Not seeing it.
  ![gitlab](/images/gitlab-304.png)


###Exercise-8
#### gitpush.sh script for nxtoolkit repository.
1. Using `ATOM` copy the `gitpush.sh` file from `ansible` folder.
2. Paste it in the `nxtoolkit` folder.
3. Switch back to the terminal window.
2. you should be in the `nxtoolkit` directory  (`training/nxtoolkit`)
3. make the gitpush.sh script executable.
4. `chmod +x gitpush.sh`
7. We will use this script in the next exercise

##Creating `nxtoolkit` repository on gitlab.
###Exercise-1
#### Create nxtoolkit project
1. login to the gitlab using your own account.
2. create a new `project`, name it `nxtoolkit`

    ![gitlab](/images/gitlab-507.png)

###Exercise-2
#### Putting nxtoolkit directory under git Repository
1. Switch to the terminal window
2. `pwd`  <-- make sure you are in the nxtoolkit directory
  2. `cd training`
  3. `cd nxtoolkit`
  4. `touch README.md`
3. Follow the instructions from the gitlab site to put our nxtoolkit directory under git version control.
4. Do not copy and paste everything at once.  Do line by line.
5. For git commit command, use -m flag.  `git commit -m 'your message'`
5. Modify the first line as follows. Add your username and password to the url ..hemakuma:cisco123@

  `git remote add origin http://hemakuma:cisco123@192.168.99.100/hemakuma/nxtoolkit.git `

  ![gitlab](/images/gitlab-508.png)
6. Switch to gitlab browser and verify that files have been uploaded.


## Creating gitlab users.
### Exercise 1
### Create another gitlab account (`developer`)
You will use the 2nd account to review the changes. You will use your first account to request the merge and using 2nd account to review , approve and merge the changes to the trunk.

1. sign out from gitlab.
2. Fill in the form for `New User` account

  ![gitlab](/images/gitlab-400.png)

3. Sign back in gitlab using the new account.  Make sure it works.

###Exercise 2
####Giving access to your projects to this user.
1. Sign out as developer account and log back in using your regular account
2. Give access to both `ansible` and `nxtoolkit` to the `developer` user.
3. Select the `ansible` project and the click on `setting`.

  ![gitlab](/images/gitlab-510.png)
4. Click on `Members`

  ![gitlab](/images/gitlab-511.png)

5. Repeat the process for the `nxtoolkit` repository

  ![gitlab](/images/gitlab-512.png)

##Adding project to ATOM
With git-plus plugin, you can manage git updates directly from ATOM.  In order to do this, you need to open each git repository as ATOM project.
###Exercise 1
####Adding git Projects in ATOM
1. Switch to `ATOM` Editor
2. Close all the open files.
2. Right click on the `training folder`
3. Click on `Remove Project Folder`

  ![gitlab](/images/gitlab-509.png)

4. Go to `File`  and `Add project Folder`
5. Navigate to `ansible`  folder under `training` and open it.
6. Repeat the same for `nxtoolkit` folder.
7. click on any file inside nxtoolkit folder and take a look at the bottom right corner. It should tell you will git branch.
8. Now u can run git add / commit / push directly from ATOM.  Go to Package menu and select git-plus.

*You want to this do way so that `ATOM` can track your git changes.  Each git repo should be a `atom project`*


## Branching

###Exercise 1
#### Creating a git branch
1. Switch to terminal window
2. Navigate to `ansible` folder. `cd training` ; `cd ansible`
4. Type `git branch`  to see what branches you currently have and where the `HEAD` is pointing to.  You should only see the `master` branch and the `HEAD` should be pointing to it.
5. Create a new branch called `readmeupdate`
5. `git branch readmeupdate`  to create a new branch.
6. `git checkout readmeupdate` to switch to new branch.
7. `git branch`  now the HEAD should be pointing to the new branch.

  ![branch](/images/branch-1.png)

8. Now lets make some changes to the `README.md` file.

###Exercise 2
#### Editing README.md file
1. Switch to `ATOM` Editor.
2. Go to `ansible` folder and open up `README.md` file.
3. Look at the right hand bottom corner to see which branch you are on. (this only works if you have git-plus plugin installed)
3. Edit the file. Put some text in it eg . `This is my 3rd edit using branch feature`.
4. `CMD+S` to save the file

  ![branch](/images/branch-1.png)

###Exercise 3
#### Push the branch to remote repository (gitlab)
1. Switch back to terminal window.
2. you should be in the `ansible` directory
3. push your changes to gitlab.  you want to push your branch to the gitlab.
4. you should still be in your `readmeupdate` branch.  Verify `git branch`
5. `git add .`  to add the changes
6. `git commit -m  'updated readme file' ` to commit the file
7. `git push -u origin readmeupdate` push the branch to gitlab


###Exercise 4
#### Create `Merge Request` on gitlab
1. Switch to the chrome browser.
2. Login to gitlab using your regular account.
3. Go to your `ansible` repository.

  ![branch](/images/gitlab-513.png)

4. you should see `Create Merge Request` button. Click on it
5. fill in the form as below and click on the `submit` button at the bottom.

  ![branch](/images/gitlab-514.png)


###Exercise 5
#### Merge the branch with master branch
1. Log out of gitlab accounts
2. log back in using `developer`  account.
3. Select `ansible` repository.
4. Click on the menu button

  ![branch](/images/gitlab-515.png)

5. Click on th `Merge Request`
6. Then select the `readmeupdate` merge request.
7. Click on the `Changes`  tab.  Take a look at all the changes that happen to the file line by line.
8. Add your comment to line 2.
8. Click on the `Discussion` tab.  
9. Write your review in the section below.
7. Then on the top, click `Accept Merge Request`

  ![branch](/images/gitlab-516.png)

###Exercise 6
####Verify the merge
1. Switch back to your terminal window
  2. make sure you are in the master branch
  3. `git branch`
  3. `git checkout master`
4. Switch to `ATOM` Editor.
  5. Take a look at the `README.md` file.  Notice that you do not see the changes that you made.
6. Switch back to terminal window.
  7. pull down the changes from your central repository
  7. `git pull`
8. Switch back to `ATOM`
  9. view the `README.md` file.
  10. Do you see the changes that got merged?

**This is how you can do simple peer reviews on your configuration files.**

###Exercise 7
#### Remove the branch
1. switch to the terminal window.
2. `git branch`
2. `git branch -d readmeupdate`
3. `git branch`
3. this should remove the branch.  We don't need it as we have merged our changes to the master (trunk)



-----

### Summary Steps

1. Clone project
2. git clone git@example.com:project-name.git
3. Create branch with your feature
4. git checkout -b $feature_name
5. Write code. Commit changes
6. git commit -am "My feature is ready"
7. Push your branch to GitLab
8. git push origin $feature_name
9. Review your code on commits page
10. Create a merge request
11. Your team lead will review the code & merge it to the main branch

### Some useful git command

```
git log
git config -l
git status
git show

```
