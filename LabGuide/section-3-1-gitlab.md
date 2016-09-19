#gitlab

### Exercise 1
### Create another gitlab account
You will use the 2nd account to review the changes.  You will pretend to be the reviewer.

1. sign out from gitlab.
2. Fill in the form for `New User` account

![gitlab](/images/gitlab-400.png)

3. Sign back in gitlab using the new account.  Make sure it works.


one for yourself

developer1
reviewer1

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
