#NXOS Onbox Programmability Guide
---
##EEM
##Scheduler
##Python
##Guestshell (optional)
###Exercise 1 - Playing with guestshell
##Bash Shell (optional)
###Exercise 1:  Enable bash shell
###Exercise 2:  Using with bash shell
##Sandbox
###Exercise 1:  Enabling NXAPI on your NXOSv Switch
###Exercise 2: Accessing and using the NXAPI using Sandbox built inside the switch.
###Exercise 3:  Show command from Sandbox.
###Exercise 4: Configuring VLAN 999 using Sandbox
###Exercise 5:  Generating Python Code.


## Guestshell (optional)

Refer to the Cisco live lab guide here
http://ngne.ciscolive.com/



### Exercise 1 - Playing with guestshell
1. Run command in guest shell
2. Make sure your NXOSv switch is running on VMware fusion or VMware Player. For more details see  NXOSv on VM Fusion or  NX-OSv in VMWare Player on Windows 7
3. Connect to the switch using serial port and get the ip address of the switch.

Open a terminal window (or putty) and Login to the switch using ssh
ssh admin@192.168.102.128   (password = cisco123)
guestshell
This will get you inside the guestshell
Try your favorite linux commands in guest shell
pwd
ip add
ip link
ethtool -S Eth1-1  <--shows the statistics of the port
Type "exit" to exit the guestshell
## Scheduler
<skip this>
This is covered in Cisco live labs.  See the jive link for more details

#EEM

See cisco EEM


## Python

## Bash Shell (optional)
Bash Shell
### Exercise 1:  Enable bash shell
1. ssh to your switch   ssh admin@<switchip>
2. config t
3. feature bash
4. run bash to get in the bash terminal.

### Exercise 2:  Using with bash shell
1. Run some linux networking commands.
2. ifconfig
3. ip link
4. yum info bgp
5. rpm -qa | grep bgp
6. Set the mtu of interface Eth1/1 to 9000
check the current mtu
ifconfig Eth1-1 | grep MTU
change the mtu
sudo ifconfig Eth1-1 mtu 9216
verify
ifconfig Eth1-1 | grep MTU
Exercise 3:  Practical Example
using bash shell, print all the interface that admin up
vsh -c "show interface brief" | grep up | awk '{print $1}'
Exit bash shell by typing  "exit"
