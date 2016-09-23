
#Ansible - Using Ansible to manage NXOS devices
Ansible is an open-source software platform for configuring ,managing and orchestrating compute and switching infrastructure. Ansible features a state-driven resource model that describes the desired state of computer systems and services.  Ansible is agentless and uses push model while puppet and chef are pull model using agents on the host.  Ansible uses YAML to configure playbooks.
Playbooks contains one or multiple plays and plays contains one or multiple tasks.  Tasks talks to ansible module. Ansible module are python scripts that configures the devices in the inventory  list.   Playbook is applied to host inventory file.  Inventory file contains list of all the devices that needs to be automated. Within a single play, you can specify which host this play applies to  from the list of all available host defined in the inventory file.

![ansible-0](/images/ansible-0.png)

![ansibel-1](/images/ansible-1.png)



**Hosts:** Remote machines Ansible manages.  
**Groups:** Group of hosts assigned to a pool that can be conveniently targeted and managed together.  
**Inventory:** File describing the Hosts and Groups in Ansible. what groups a host belongs to, the
properties those groups and hosts have.
**Modules:** Modules (also referred to as “task plugins” or “library plugins”) are the components that do the actual work in Ansible. They are what gets executed in each playbook task.  
**Playbooks:** A collection of plays which the Ansible Engine orchestrates, configures, administers, or deploys. These playbooks describe the policy to be executed to the host(s).  People refer to these playbooks as "design plans" which are designed to be human- readable and are developed in a basic text language called **YAML**.
**Play**: specify a list of tasks that are run in sequence across one or more hosts. Each task
can also run multiple times with a variable taking a different value. Playbooks are expressed in
YAML format
**Roles**: are a way to encapsulate common tasks and properties for reuse, if you find yourself writing
the same tasks in multiple playbooks, turn them into roles.

http://docs.ansible.com/ansible/list_of_network_modules.html



### Exercise 1
#### Setting up the directory structure to host ansible files
1. Switch to `ATOM` Editor
2. Right click on the `ansible` folder
3. Select new folder
4. name it `roles`
5. Repeat above and create following folders.
    6. templates
    7. groups_vars
    8. hosts_vars

### Exercise 3
####Creating Host Inventory File
Inventory file contains list of hosts that you want to manage from Ansible.  In our case, it will list of switches that we want to manage by ansible. These host/swiches can be organized in groups.

2. In the `ATOM` Editor , right click on `ansible` folder and select `New File`.
3. Save this file as `hosts`
3. add the the following lines to the file   

    ```
    [all:vars]
    ansible_connection = local

    [leafs]
    n9k-1

    ```
4. Delete all the host from the above file , except for the switch that is assigned to you.  Your host file should be similar to the picture below.

    ![ansibel-1](/images/ansible-2.png)

5. We are using  ansible connection as local rather than ssh, because the nxos ansible modules use nxapi to configure the switches rather than ssh.
6. Save the file `CMD + S`

You can read more about Inventory file here:  Inventory http://docs.ansible.com/ansible/intro_inventory.html

### Exercise 4
####Creating credentials file
1. Under `ansible` folder , create a new file
2. name it `credentials.yml`
3. copy and paste the following.

    ```
    ---
    creds:
        host: "{{ inventory_hostname }}"
        username:   admin
        password:   cisco123
    ```
4. Save the file `cmd + S`

To login into the switches, we need the `username` and `password`.  This file with combination with `provider` keyword in ansbile playbook will let ansible login into the devices.  

**All core networking modules implement a provider argument, which is a collection of arguments used to define the characteristics of how to connect to the device.
The provider argument accepts keyword arguments and passes them through to the task to assign connection and authentication parameters.**
https://docs.ansible.com/ansible/intro_networking.html

### Exercise 5
####Ping Test.  Make sure your Switch can reach internet.
You need to get yourself familiarize with nxos ansible modules.  Take a look at the ping module.  
http://docs.ansible.com/ansible/nxos_ping_module.html

1. Under `ansible` folder , create a new file
3. name it `ping.yml`
4. copy and past the following :

    ```
    ---
    - name: ping testing
      hosts: n9k-1
      connection: local
      gather_facts: no
      tasks:
        - name: obtain login credentials
          include_vars: credentials.yml

        # test reachability to 8.8.8.8 using mgmt vrf
        - nxos_ping: dest=8.8.8.8 vrf=management host={{ inventory_hostname }} provider="{{ creds }}"

        # Test reachability to a few different public IPs using mgmt vrf
        # if device has name lookups turned on, you can use names
        - nxos_ping: dest={{ item }} vrf=management username=admin password=cisco123 host={{ inventory_hostname }}
          with_items:
            - 8.8.8.8
            - 4.4.4.4

    ```

9. `CMD+S` to save it

Note:  Environment variables can be set at the play or task level.
http://docs.ansible.com/ansible/faq.html  

##Exercise 6
####Login in to your Ansible Container and Run the Playbook
1. Go to your terminal window and type the following:
    ```
    cd training
    source source.docker
    docker attach ansible
    ```
2. you should be inside your ansible docker container.
3. update the hosts file on the ansible container. Since we not using dns for host resolution, we need to update the hosts file on the container
    1. insid the ansible container type
    2. `vim /etc/hosts`
    3. press `i` to insert text
    4. add a new entry to reflect all your hosts

        ![hosts](/images/ansible-300.png)
    5. Save the file ...`esc`  then type `:wq`
    6. `ping n9k-1`  make sure the host is able to resolve it.
3. Run Ping playbook.
    1. `ansible-playbook -i hosts ping.yml`




###Exercise 6 (don't do this WIP)
####Using the nxos_config module
Cisco NXOS configurations use a simple block indent file syntax for segmenting configuration into sections
http://docs.ansible.com/ansible/nxos_config_module.html

```
---
- name: snmp access-list
  hosts: n9k-1
  connection: local
  gather_facts: no
  tasks:
    - name: obtain login credentials
      include_vars: credentials.yml

    - nxos_config:
        lines:
          - 5 permit ip 10.132.243.9/32 any
          - 10 permit ip 10.241.16.91/32 any
          - 20 permit ip 140.84.159.66/32 any
          - 30 permit ip 140.84.54.128/26 any
          - 40 permit ip 152.69.77.16/28 any
          - 50 permit ip 152.69.77.32/28 any
          - 60 permit ip 10.153.162.0/23 any
          - 70 permit ip 10.153.162.0/23 any
          - 80 permit ip 10.153.162.0/23 any
          - 90 permit ip 10.153.162.0/23 any
          - 240 deny ip any any
        parents: ip access-list 50-SNMP-Access
        before: ip access-list 50-SNMP-Access
        match: exact
        provider: "{{ creds }}"

```

###Exercise 7
####Using the nxos_template module

###Exercise 8
####Creating the data file
1. Switch back to the `ATOM` editor
2. Right click on the `ansible` folder and select `New File`
3. name the folder `base-vars.yml`
4. copy and paste the following in the file.

    ```
    snmp_acl:
        - 10.241.16.91/32
        - 10.132.243.9/32
        - 152.69.77.32/28
    ```
5. Save the file `cmd + S`

###Exercise 9
####Using Creating Jinja2 template
1. Go to `Ansible` folder and then to `templates` folder
2. Right click on the `templates` folder and select `New File`
3. name the file `basetemplate.j2`
4. copy and paste the following:

    ```
    #snmp acl configuration
    no ip access-list 50-SNMP-Access
    ip access-list 50-SNMP-Access
       {% for sourceip in snmp_acl %}
       permit ip {{ sourceip }} any
       {% endfor %}
       deny ip any any
    ```
7. Save the file `cmd + S`


###Exercise 10
####Creating playbook to configure switch using jinja2 template
In this exercise, we will be using the data file (base-vars.yml) and jinja2 template (basetemplate.j2) to configure multiple snmp access list.  

This playbook will use the data file (base-vars.yml) and render those data into the jinja2 template (basetemplate.j2). The playbook (baseconfig.yml) calls this jinja2 template via ansible  nxos template module.

This modules provides a way to push a set of commands onto a network device by evaluating the current running-config and only pushing configuration commands that are not already configured. The config source can be a set of commands or a template.

2. Using `ATOM` create a new playbook. Right click on the `ansible` folder and select `New File`
3. name it `baseconfig.yml`
4. It should look like this

    ```
    ---
    - name: template testing
      hosts: n9k-1
      connection: local
      gather_facts: no
      tasks:
        - name: obtain login credentials
          include_vars: credentials.yml
        #contains all the base configuration data
        - name: getting the data file
          include_vars: base-vars.yml

        #renders the jinja2 template
        - name: configure base template
          nxos_template:
            provider: "{{ creds }}"
            src: basetemplate.j2
    ```

5. you can take a look at my file here:   https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/baseconfig.yml


#### Running the playbook
1. Switch to ansible container terminal
2. cd to ansible folder and run the playbook
3. `ansible-playbook -i hosts baseconfig.yml`


#### Verifying the configuration on the switches
1. switch to ssh session to the switch
2. `show run`
3. verify the configuration

###Exercise 11
####Adding more base configuration
1. update your data file with other data that you need to have in your base configuration.
2. Here is my list, you can add anything u like.
3. https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/base-vars.yml

![ansible2](/images/ansible2-1.png)

####Updating the jinja2 template
1. From the 'Atom' editor
2. open `basetemplate.j2`
3. update the template to reflect what u want in your configuration.
4. It should look similar to this.

    ![ansible2](/images/ansible2-2.png)

5. Take a look at my template here:  https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/basetemplate.j2

#### Run the playbook again
1. Switch to the ansible terminal
2. `ansible-playbook -i hosts baseconfig.yml`

#### Verify the configuration on the switches
1. switch to ssh session to the switch
2. `show run`
3. verify the configuration

###Exercise 12
####Base Configuration on switches
Ensure the all the switches in the DC has following base configuration


ntp server 10.68.0.41 use-vrf management



### Exercise 14
###Configure ipv4 address to the interface
Configure interface eth1/1 with 10.1.100.2/24

1. Switch to your ansible terminal
2. Lets make a copy of this script.
3. cp examples-ipv4interface.yml  hk-ipv4interface.yml
4. Switch to ATOM editor
5. double click the file and edit it. Modified as per requirement.  you will have to delete few things and make sure
http://gitlab.cisco.com/hemakuma/se-training/blob/master/ansible/how-to/hk-ipv4interface.yml
6. Switch to the Switch CLI terminal
7. Verify that no ip address is configured on eth1/1
8. show run interface
9. Switch to ansible terminal
10. run the playbook
11. ansible-playbook -i hosts hk-ipv4interface.yml
12. Repeat 3 and compare the result.

### Exercise 15
####Show neighbors

1. Switch to your ansible terminal
2. Lets make a copy of this script.
3. `cp get-neighbors.yml  hk-get-neigbors.yml`
4. Switch to ATOM windows
5. edit the file
6. modify the source and destination for the template.
http://gitlab.cisco.com/hemakuma/se-training/blob/master/ansible/how-to/hk-get-neigbors.yml

7. This tells ansible to use the jinja2 template named neigbhors.j2 located at /nxos-ansible/templates/ folder to format the output.  It also tells ansible to  place the out in the /nxos-ansible/myscripts/ folder and name it neighbors.json.
register saves the output in a variable called my_neighbor.   Read more about register variable here  Conditionals — Ansible Documentation
click  ansible --> templates --> neighbors.j2
notice how the register variable is used in the template.
8. Switch to ansible window
9. run the playbook
10. `ansible-playbook  -i hosts hk-get-neigbors.yml`
11. Switch to ATOM Editor
12. Examine the output
13. click on ansible folder.
14. you should see `neighbors.json` file.  Double click it and examine the output.


#Roles
Simply put, roles are a further level of abstraction that can be useful for organizing playbooks. As you add more and more functionality and flexibility to your playbooks, they can become unwieldy and difficult to maintain as a single file. Roles allow you to create very minimal playbooks that then look to a directory structure to determine the actual configuration steps they need to perform.

Organizing things into roles also allows you to reuse common configuration steps between different types of servers. This is already possible by "including" other files within a playbook, but with roles, these types of links between files are automatic based on a specific directory hierarchy.

In general, the idea behind roles is to allow you to define what a server is supposed to do, instead of having to specify the exact steps needed to get a server to act a certain way.

##Base Configuration
Applying the base configuration to all switches in the inventory using jinja2 template.  Templates are good for mostly static configuration.  Since base configuration does not change that often, it is good idea to put them in a template.

###Exercise 1
####Create ansible roles directory structure
1. Switch to 'ansible container'
2. cd to roles directory
3. type `ansible-galaxy init login`
4. type `ansible-galaxy init baseconfig`
5. type `ansible-galaxy init vlans`
6. type `ansible-galaxy init uplinks`
7. type `ansible-galaxy init hostports`

###Exercise 2
####Configuring roles
1. Switch to `Atom` Editor
2. go to `login` folder under `roles`.
3. expand it
4. navigate to `vars` folder and open `main.yml`
5. type in the following:

    ```
    ---
    creds:
        host: "{{ inventory_hostname }}"
        transport: nxapi
        username:   admin
        password:   cisco123
    ```

    ![ansiblerole](/images/ansiblerole-1.png)

6. Save the file `Cmd+S`

###Exercise 3
#### Base configuration repository
We created a role to hold all the base configuration data.  Base configuration includes things like ntp, snmp, alias etc that needs to be  provisioned to all switches.

1. Navigate to `ansible --> roles --> baseconfig --> vars`
2. Open up the `main.yml` file
3. copy and paste the following code.
4. https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/base-vars.yml
4. Save the file `Cmd+S`
5. Your file should look like this.

    ![ansiblerole](/images/ansible-301.png)


###Exercise 3
#### Create jinja2 template for the base configuration.

nxos_template manages network device configurations over SSH or NXAPI. This module allows implementers to work with the device running-config. It provides a way to push a set of commands onto a network device by evaluating the current running-config and only pushing configuration commands that are not already configured. The config source can be a set of commands or a template.

**What is Jinja2 Template?**

Jinja2 is a very popular and powerful Python-based template engine. Templates look very similar to normal text-based files except for the occasional variables or code that surrounds the special tags. These get evaluated and are mostly replaced by values at runtime, creating a text file, which is then copied to the destination host. The following are the two types of tags that Jinja2 templates accept:

`{{ }}` embeds variables inside a template and prints its value in the resulting file. This is the most common use of a template.
For example: `{{ hostname }}`

`{% %}` embeds statements of code inside a template, for example, for a loop, it embeds the if-else statements, which are evaluated at runtime but are not printed.

So where does jinja2 gets the values for its variables?  At the runtime of the playbook, Jinja2 has access to all the variable that is reference inside that particular playbook.  So the variables inside the jinja2 template is substituted with the values from the facts and variable obtained from the playbook at the run time.


**Steps to create the templates:**

1. Always start with the actual configuration file.
2. Identify the parameters that needs to be dynamically generated
3. create a template. Add variables as you see fit.
4. create the variable file.
5. create the playbook
6. run the playbook

---

1. Navigate to `ansible --> roles --> baseconfig --> templates`
2. Right click on the `templates` folder and select `New File`. Name it `basetemplate.j2`
3. copy and paste the content from this link.  This is the jinja 2 template.

    https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/basetemplate.j2

4. Save the file `CMD + S`

###Exercise 4
#### Create handler to save the configuration
1. Navigate to `ansible --> roles --> baseconfig --> handlers`
2. Open up the `main.yml` file
3. Copy and paste the following
    ```
    ---
    # handlers file for baseconfig
    - name: Save Config
      nxos_config:
        provider: "{{ creds }}"
        lines:
          - 'copy run start'
    ```
4. Save the file `CMD + S`

###Exercise 5
#### Create tasks to create a template for base configuration.
1. Navigate to `ansible --> roles --> baseconfig --> tasks`
2. Open up the `main.yml` file
3. Copy and paste the following:

    ```
    ---
    # tasks file for baseconfig
    #renders the jinja2 template

    - name: configure base template
      nxos_template:
        provider: "{{ creds }}"
        src: basetemplate.j2
        backup: yes
      notify:
        - Save Config
    ```
4. Note we are specifying the jinja2 template to be used to build this baseconfig file.  Also we are going to back the configuration prior to making any changes.  Finally we will save the configurtion.
5. Save the file `CMD + S`


###Exercise 6
#### Create playbook to push this base configuration to the switch.
1. Navigate to `ansible` folder
2. Right click and select `New File`. Name it `deploy-baseconfig.yml`
3. Copy and paste the following:

    ```
    ---
    - hosts: all
      connection: local
      strategy: free
      roles:
        - { role: login, tags: [ 'login' ] }
        - { role: baseconfig, tags: [ 'login', 'base'] }
    ```
4. Save the file `CMD + S`

###Exercise 7
#### Lets run the playbook
1. Switch to the `ansible container` terminal window.
2. Run the playbook
    1. `ansible-playbook -i hosts deploy-baseconfig.yml`
3. Login into your switch.
4. Verify that ansible has made those configuration.

###Excercise 8
#### New NTP server
Lets say Server guys added a new `NTP server` which has ip of `192.200.0.2`. You want to update all your switches in your DC to reflect this change.  Today, you might be logging into all the switches and manually typing this in.  With ansible, we to go one file (the variables file) and make this modification.  Then we run the playbook again.  Within secs , it will update your entire DC switches with the new ntp server information.  Note, ansible is idempotent, therefore it will not change anything else except that one small change.  Therefore this is not be disruptive change.

1. Switch to `ATOM` editor
    2. Navigate to `ansible --> roles --> baseconfig --> vars`
    2. Open up the `main.yml` file
    3. add this a line to your `NTP server list`
    4. `- 192.200.0.2`
2. Switch to the ansible terminal window
    1. Run the playbook again
    2. `ansible-playbook -i hosts deploy-baseconfig.yml`
3. Login into your switch.
    1. Verify that ansible has made those configuration.
4. Switch to `ATOM` editor.
    1. Navigate to `ansible --> roles --> baseconfig-->backup`
    2. you should see your backupfiles


----
## VLAN configuration

### Exercise 13
####Practical Example - Automating VLAN provisioning
**Problem Statement**
Users perform manual Day-2 operations to configure network elements repeatedly for the purpose of on-boarding new workloads. One common category of Day-2 configuration activity is performing routine VLAN related operations:
1. Check if VLAN exists
2. Change names and descriptions of VLANs
3. Configure a VLAN

It would be beneficial to automate this repetitive configuration with a goal of reducing time and effort needed to complete the configurations, while reducing the probability of errors.

**Solution  - Ansible automation**
Ensure following vlans are configured on all the switches.

| Vlan id |	vlan name |
|---------|-----------|
|10	| web|
| 20 |	app|
|30	|db|
|40	|misc
|99	|native-vlan




#### vlan configuration repository
We created a role to hold all the vlan configuration data. Lets modify it to meet the above requirement.

Lets update the vars directory with all our variables.

1. Navigate to `ansible --> roles --> vlans --> vars`
2. Open `main.yml`
3. copy and paste the following code.

    ```
    vlans:
        - { vlan_id: 10, name: web, state: present }
        - { vlan_id: 20, name: app, state: present }
        - { vlan_id: 30, name: db, state: present }
        - { vlan_id: 40, name: misc, state: absent }
        - { vlan_id: 99, name: native_vlan, state: present }
    ```
4. Save the file `Cmd+S`

###Exercise 2
#### Create handler to save the configuration
1. Navigate to `ansible --> roles --> vlans --> handlers`
2. Open up the `main.yml` file
3. Copy and paste the following

    ```
    ---
    # handlers file for baseconfig
    - name: Save Config
      nxos_config:
        provider: "{{ creds }}"
        lines:
          - 'copy run start'
    ```
4. Save the file `CMD + S`

###Exercise 3
#### Create playbook tasks to configure the switch ports.
This time, we are going to use `nxos_vlan` module to make these changes.  Read more about it here
https://docs.ansible.com/ansible/nxos_vlan_module.html

1. Navigate to `ansible --> roles --> vlans --> tasks`
2. Open up the `main.yml` file
3. Copy and paste the following:

    ```
    ---
    -   name: configuring the vlans
        nxos_vlan: vlan_id={{ item.vlan_id }}  name={{ item.name }} state={{ item.state }}  provider="{{ creds }}"
        with_items: "{{vlans}}"
        notify:
          - Save Config
    ```
5. Save the file `CMD + S`


###Exercise 6
#### Create playbook to push host port configuration to the switch.
1. Navigate to `ansible` folder
2. Right click and select `New File`. Name it `deploy-vlans.yml`
3. Copy and paste the following:

    ```
    ---
    - hosts: all
      connection: local
      strategy: free
      roles:
        - { role: login, tags: [ 'login' ] }
        - { role: vlans, tags: [ 'login', 'vlans'] }
    ```
4. Save the file `CMD + S`

###Exercise 7
#### Lets run the playbook
1. Switch to the `ansible container` terminal window.
2. Run the playbook
    1. `ansible-playbook -i hosts deploy-vlans.yml`
3. Login into your switch.
4. Verify that ansible has made those configuration.


----


## Uplink port Configuration.
In this section, we will be configuring uplink ports. This will include changing the interface to L3 mode and then assigning ip to it.  We are going to use jinja2 template for this.

###Exercise 1
#### Uplinks configuration repository
We have created a role to hold all the uplink configuration data.  

Since the config is per switch basis, we need to hold the variables in the `host_vars`. We need create folder for each host in this folder.  Ansible will search this folder to look for the variables.

1. Navigate to `ansible --> hosts_vars`
2. Right click and select `New Folder`. Name it `n9k-1.yml`
3. copy and paste the following code.

    ```
    ---
    uplink_interface:
      - {int: "ethernet1/1", ip: 192.168.1.2/30}
      - {int: "ethernet1/2", ip: 192.168.2.2/30}

    ```
4. Save the file `Cmd+S`

###Exercise 2
#### Create handler to save the configuration
1. Navigate to `ansible --> roles --> uplinks --> handlers`
2. Open up the `main.yml` file
3. Copy and paste the following

    ```
    ---
    # handlers file for baseconfig
    - name: Save Config
      nxos_config:
        provider: "{{ creds }}"
        lines:
          - 'copy run start'
    ```
4. Save the file `CMD + S`

###Exercise 3
#### Create playbook tasks to configure the switch ports.
This time, we are going to use `nxos_template`  module to make these changes.  Read more about it here

https://docs.ansible.com/ansible/nxos_template_module.html

1. Navigate to `ansible --> roles --> uplinks --> tasks`
2. Open up the `main.yml` file
3. Copy and paste the following:

    ```
    ---
    # tasks file for uplinks
    - name: configure uplinks template
      nxos_template:
        provider: "{{ creds }}"
        src: interface.j2
        backup: yes
      notify:
        - Save Config

    ```
5. Save the file `CMD + S`


###Exercise 6
#### Create playbook to push host port configuration to the switch.
1. Navigate to `ansible` folder
2. Right click and select `New File`. Name it `deploy-uplinks.yml`
3. Copy and paste the following:

    ```
    ---
    - hosts: all
      connection: local
      strategy: free
      roles:
        - { role: login, tags: [ 'login' ] }
        - { role: uplinks, tags: [ 'login', 'uplinks'] }
    ```
4. Save the file `CMD + S`

###Exercise 7
#### Lets run the playbook
1. Switch to the `ansible container` terminal window.
2. Run the playbook
    1. `ansible-playbook -i hosts deploy-hostports.yml`
3. Login into your switch.
4. Verify that ansible has made those configuration.




----
## Hostport Configuration.
Another common Day 2 operations tasks is to configure hosts/server ports.  We want to have consistent configuration on all the server facing ports.

###Exercise 1
#### Hostport configuration repository
We have created a role to hold all the hostport configuration data.  Host port configuration includes things like `switch port access mode`,  etc that needs to be  provisioned to per switches basis.

Since the config is per switch basis, we need to hold the variables in the `host_vars`. We need create folder for each host in this folder.  Ansible will search this folder to look for the variables.

1. Navigate to `ansible --> hosts_vars`
2. Right click and select `New Folder`. Name it `n9k-1.yml`
3. copy and paste the following code.

    ```
    ---
    hostports:
       - {int: "ethernet1/5", des: ESXI-1}
       - {int: "ethernet1/6", des: ESXI-2}
       - {int: "ethernet1/7", des: Openstack-nova-server-1}
       - {int: "ethernet1/8", des: Openstack-nova-server-2}
    ```
4. Save the file `Cmd+S`

###Exercise 2
#### Create handler to save the configuration
1. Navigate to `ansible --> roles --> hostports --> handlers`
2. Open up the `main.yml` file
3. Copy and paste the following

    ```
    ---
    # handlers file for baseconfig
    - name: Save Config
      nxos_config:
        provider: "{{ creds }}"
        lines:
          - 'copy run start'
    ```
4. Save the file `CMD + S`

###Exercise 3
#### Create playbook tasks to configure the switch ports.
This time, we are going to use `nxos_interface`  and `nxos_switchport` module to make these changes.  Read more about it here

https://docs.ansible.com/ansible/nxos_interface_module.html
https://docs.ansible.com/ansible/nxos_switchport_module.html

1. Navigate to `ansible --> roles --> hostports --> tasks`
2. Open up the `main.yml` file
3. Copy and paste the following:

    ```
    ---

    # tasks file for hostports
    - name: Ensure that port is in layer 2 mode
      nxos_interface:
        provider: "{{ creds }}"
        interface: Ethernet1/5
        description: 'Configured by Ansible'
        mode: layer2

    - nxos_switchport:
        provider: "{{ creds }}"
        interface: eth1/5
        mode: access
        access_vlan: 20
      notify:
        - Save Config
    ```
5. Save the file `CMD + S`


###Exercise 6
#### Create playbook to push host port configuration to the switch.
1. Navigate to `ansible` folder
2. Right click and select `New File`. Name it `deploy-hostports.yml`
3. Copy and paste the following:

    ```
    ---
    - hosts: all
      connection: local
      strategy: free
      roles:
        - { role: login, tags: [ 'login' ] }
        - { role: hostports, tags: [ 'login', 'hostports'] }
    ```
4. Save the file `CMD + S`

###Exercise 7
#### Lets run the playbook
1. Switch to the `ansible container` terminal window.
2. Run the playbook
    1. `ansible-playbook -i hosts deploy-hostports.yml`
3. Login into your switch.
4. Verify that ansible has made those configuration.

###Excercise 8
#### New NTP server ????
Lets say Server guys added a new `NTP server` which has ip of `192.200.0.2`. You want to update all your switches in your DC to reflect this change.  Today, you might be logging into all the switches and manually typing this in.  With ansible, we to go one file (the variables file) and make this modification.  Then we run the playbook again.  Within secs , it will update your entire DC switches with the new ntp server information.  Note, ansible is idempotent, therefore it will not change anything else except that one small change.  Therefore this is not be disruptive change.

1. Switch to `ATOM` editor
    2. Navigate to `ansible --> roles --> baseconfig --> vars`
    2. Open up the `main.yml` file
    3. add this a line to your `NTP server list`
    4. `- 192.200.0.2`
2. Switch to the ansible terminal window
    1. Run the playbook again
    2. `ansible-playbook -i hosts deploy-baseconfig.yml`
3. Login into your switch.
    1. Verify that ansible has made those configuration.
4. Switch to `ATOM` editor.
    1. Navigate to `ansible --> roles --> baseconfig-->backup`
    2. you should see your backupfiles


##Customer User Cases

###Exercise 1
####Configuring roles
  base config automation (e.g. AAA, syslog/snmp, acl 50, NTP, etc)  for Cisco

Here is the Customers base configuration that they like to automate using ansible.

```
ntp server 10.68.0.41 use-vrf management
ntp server 10.68.0.42 use-vrf management
ntp server 10.246.0.1 use-vrf management
ntp server 10.246.0.2 use-vrf management
ntp server 144.20.15.229 use-vrf management
ntp server 144.20.15.230 use-vrf management
ntp source-interface  mgmt0

ip access-list 50-SNMP-Access
  5 permit ip 10.132.243.9/32 any
  10 permit ip 10.241.16.91/32 any
  20 permit ip 140.84.159.66/32 any
  30 permit ip 140.84.54.128/26 any
  40 permit ip 152.69.77.16/28 any
  50 permit ip 152.69.77.32/28 any
  60 permit ip 10.153.162.0/23 any
  70 permit ip 10.153.162.0/23 any
  80 permit ip 10.153.162.0/23 any
  90 permit ip 10.153.162.0/23 any
  100 permit ip 148.87.43.0/24 any
  110 permit ip 141.146.7.0/25 any
  120 permit ip 148.87.109.0/25 any
  130 permit ip 10.177.40.45/32 any
  140 permit ip 10.172.212.62/32 any
  150 permit ip 192.135.82.116/32 any
  160 permit ip 10.165.246.75/32 any
  170 permit ip 148.87.13.32/27 any
  180 permit ip 139.185.121.0/24 any
  190 permit ip 10.167.208.87/32 any
  200 permit ip 10.221.65.11/32 any
  210 permit ip 172.17.10.3/32 any
  220 permit ip 172.17.16.35/32 any
  230 permit ip 10.222.24.144/28 any
  240 deny ip any any

radius-server host 10.230.86.165 key 7 “radiuskey” authentication accounting
radius-server host 152.69.76.90 key 7 “radiuskey" authentication accounting

aaa group server radius pdit_radius
    server 10.230.86.165
    server 152.69.76.90
    use-vrf management
    source-interface mgmt0


cli alias name wri copy run start
cli alias name q exit
cli alias name swt switchto vdc
cli alias name wr copy run start
line console
  exec-timeout 10
line vty
  session-limit 15
  exec-timeout 10

ip radius source-interface mgmt0

logging server 144.20.94.2 5 use-vrf management
logging server 10.222.24.82 5 use-vrf management
logging server 10.236.130.4 5 use-vrf management
logging timestamp microseconds
logging monitor 7
logging level syslog 5
logging level local7 6
no logging console

```


#Ansible Tips
###How to see which ansible modules are installed?
ansible-doc --list | egrep ^'nxos'
