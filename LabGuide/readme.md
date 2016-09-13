## Introduction
This lab is focusing on getting you started with the basic on nexus open-nxos  programmability.  We will install and use some of the common tools used by software engineers.  This lab will build the foundation for future advance labs.
![intro](/images/intro-1.png)
Slides used in this lab is on the box.  https://cisco.box.com/s/x5ets0d5wcrs1q8t3n7wihowy94v2p4s

## Objective
To teach you how to program Nexus switches using varies tools/apps via RESTful API calls.  At the end of the day, each tool changes the managed object stored inside the data model.  In this lab we will be using CLI, REST Client, Python, nxtoolkit and ansible to make configuration changes to the switches.

![intro](/images/intro-2.png)

## Lab Setup
For this lab, we will be using N9K NXOSv switch.  You need to have this virtual switch installed on your laptop.  If you have not already installed it,  see  the prerequisite section above for the details on how to install NXOSv it on your laptop.

For those, who for some reason can not install NXOSv on your laptop, we have installed 20 VM with NXOSv in our lab.  The IPs and hostnames are given below.

![intro](/images/intro-3.png)

Most of these tools can be installed on your laptop

![intro](/images/intro-4.png)

If you can not install NXOSv on your laptop, use of these NXOSv installed in the lab.
###IP address Assignment

|Student No.| IP Address| Device Name|
|-----------|-----------|------------|
1	|10.114.217.157	|N9K-1
2	|10.114.217.158	|N9K-2
3	|10.114.217.159	|N9K-3
4	|10.114.217.160	|N9K-4
