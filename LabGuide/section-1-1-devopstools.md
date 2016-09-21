
#DevOps Tools
Most of the developers develop applications on their laptop.  They have most of the application development tools installed on their laptop. We will try to setup similar environment for you on your laptop. This way we can use these tool for today's lab and then you can have it on your laptop to continue your learning.  Lets install some of the tools:

##Code Editor
### Exercise-1
####Installing Atom IDE
To write codes, you will need a good text editor that does syntax highlighting. Atom is excellent editor for editing  codes. It is opensource so there is no fee for it.  Any text editor with syntax highlight will do.

1. Download and install atom editor for your OS.
http://atom.io

#### Add git-plus plugins
1. Click on Atom
2. click on Preference.
3. click on `Install`
4. search for `git-plus`
5. Click install to install the package.

**Tips on using git-plus**
cmd + shift + H  <-- to get the command window

##Virtual Box
Virtual box is a x86 virtualization product.  Basically it lets you run virutal machines on your laptop.

### Exercise-2
####Installing Virtual box

1. Download and install Virtualbox for your OS.
    https://www.virtualbox.org/


### Exercise 3
####Verify Virtual box installation
1. On the terminal window type `vboxmanage -v`


## GIT
For source control.
### Exercise-1
####Installing git
##### Mac Users
1. **Xcode** is a large suite of software development tools and libraries from Apple.  This includes git package.  Lets install xcode-select, command line utility for xcode.
2. Open up a terminal window.  `Cmd+space`  and type `terminal`
3. on the terminal type  `xcode-select --install`
4. A software update popup window will appear.
5. click on `Install`.
6. Wait for it to install the Command line tools.
7. type  `git --help`
8. you see all the options for git command.

##### Windows Users
1. Download  and install `mysGit` package. Use all the default setting for installation. https://git-for-windows.github.io/
2. Use all the default during installation.
2. You should see Git Bash icon on your desktop.  

## Do not do the sections below. These tool are not needed for the lab.
##Vagrant Tool
###Exercise 4
####Installing Vagrant Tool
1. Download and install vagrant from here
    https://www.vagrantup.com/downloads.html

For more information see vagrant documentation
https://www.vagrantup.com/docs/installation/


###Exercise 5
####Verify installation of vagrant tool
Verify that it is installed correctly

1. Open a terminal window
2. Type `vagrant -v` and `vagrant -h`
3. make sure you see help options
