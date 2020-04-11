# Resources for setting up EVE-NG in the Google Compute Platform. 

### The initial steps for install of EVE-NG on GCP are detailed on the [EVE-NG website](https://www.eve-ng.net/index.php/documentation/installation/google-cloud-install/)

Credits and inspiration go-to:

* This comprehensive post by [ithitman](http://ithitman.blogspot.com/2018/04/configuring-eve-ng-on-google-compute.html)

* Tony Es similar but basic blog [post](https://showipintbri.blogspot.com/2018/08/eve-ng-in-cloud.html) which points to his comprehensive video on [YouTube](https://www.youtube.com/watch?v=HDHsMgCs0XU)

* And also watch this alternative [Azure](https://youtu.be/hdUSNWMHbUU) setup by thelantamer - see my setup guide [here](https://github.com/sipart/spring_lab/blob/master/azure/AZURE%20EVE-NG%20setup.md) for EVE-NG in Azure. I would not advise using Azure as the EVE-NG kernel is always overwritten by the MS Azure Linux kernel - trying to swap this out with the EVE-NG kernal will result in an unsable VM/EVE-NG!

### Once EVE-NG install has been completed and verified then move on to the bonus notes and steps below

### Other notes and steps for this lab setup
I used the GCP - europe-west2-c region VPC in the following example changes - 10.154.0.0/20

#### Sourcing and Uploading virtual device images

Vendor virtual images need to be sourced by yourself and uploaded to the ```/opt/unetlab/addons``` folders (dynamips or iol or qemu) - follow the folder and qemu image naming conventions detailed on the [EVE-NG website here](https://www.eve-ng.net/index.php/documentation/qemu-image-namings/). Always fix permissions (```/opt/unetlab/wrappers/unl_wrapper -a fixpermissionsthe```) the first time you copy an image or images to your EVE-NG server.

#### [Cloud9 setup on EVE-NG server](https://d-herrmann.de/2018/04/nat-cloud-in-eve-ng-community-edition/) - allows Internet access for Linux host in topology and EVE-NG/Linux host access to network device mgmt. - commands assume logged in as root. 

```
ip address add 10.154.0.10/20 dev pnet9
echo 1 > /proc/sys/net/ipv4/ip_forward
iptables -t nat -A POSTROUTING -o pnet0 -s 10.154.0.0/20 -j MASQUERADE
```

##### To make pnet9 IP persistent

Edit the interfaces file

```
nano /etc/network/interfaces
```

Edit eth9 for your preferred IP

```
    iface eth9 inet manual
    auto pnet9
    iface pnet9 inet static
        address 10.154.0.10
        netmask 255.255.240.0
        bridge_ports eth9
        bridge_stp off
```

##### To make iptables persistent

Install iptables-persistent

```
apt-get install iptables-persistent
```

Save

```
netfilter-persistent save
```

Reload

```
netfilter-persistent reload
```

reboot the system and do the following command:

```
iptables -t nat -L
```

It should show the rule as persistent

##### To make ip_forward persistent

Edit the sysctl config file

```
nano /etc/sysctl.conf
```

Uncomment 'net.ipv4.ip_forward=1' and save

Issue the following command:

```
sysctl -p /etc/sysctl.conf
```

Reboot the system and issue the following command:

```
cat /proc/sys/net/ipv4/ip_forward
```

It should show 1 in the output

Check routing on the EVE-NG server:

```
root@sipart-eve:~# route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         10.154.0.1      0.0.0.0         UG    0      0        0 pnet0
10.154.0.0      *               255.255.240.0   U     0      0        0 pnet9
10.154.0.1      *               255.255.255.255 UH    0      0        0 pnet0

root@sipart-eve:~# ip route list
default via 10.154.0.1 dev pnet0
10.154.0.0/20 dev pnet9  proto kernel  scope link  src 10.154.0.10
10.154.0.1 dev pnet0  scope link
```

#### Setup of network device mgmt. and Linux box mgmt.

Connect devices mgmt. ints (fxp0 on vMX) to cloud9 network object

Set IP on mgmt. interfaces -

10.154.0.201/20 and up for PEs

10.154.0.221/20 and up for CEs

#### Use this pre-built automation Ubuntu 18.04 Linux box image by Calin @ https://ipnet.xyz - download from here [18.04 Ubuntu server](https://ipnet.xyz/2018/06/ubuntu-image-for-eve-ng-python-for-network-engineers/)) and add to EVE-NG and add to any topology - login as pfne.

Edit the netplan file to get Linux box networked (Ubuntu 18.04 uses netplan for interface config)

```
sudo nano /etc/netplan/01-netcfg.yaml
```

```
This file describes the network interfaces available on your system
For more information, see netplan(5).
network:
 version: 2
  renderer: networkd
  ethernets:
    ens3:
     addresses: [10.154.0.200/20]
     gateway4: 10.154.0.10
     nameservers:
       addresses: [8.8.8.8, 1.1.1.1]
     dhcp4: no
```

Apply netplan changes
  
```
sudo netplan apply
```

Check if you can ping 8.8.8.8 :-)

Extras to install on the Linux box (the automation host already has Openssl, Net-tools (ifconfig..), IPutils (ping, arping, traceroute…), IProute, IPerf, TCPDump, NMAP, Python 2, Python 3, Paramiko (python ssh support), Netmiko (python ssh support), Ansible (automation), Pyntc and NAPALM installed):
```
sudo ansible-galaxy install Juniper.junos

sudo pip2 install git+https://github.com/Juniper/py-junos-eznc.git

sudo pip install git+https://github.com/networkop/ssh-copy-net.git
        
sudo pip install jxmlease
```

Create SSH key when logged in as pfne:

```
ssh-keygen -t rsa -b 2048
```

Use SSH copy util to copy SSH key of current logged in Linux host account to Juniper boxes in this case 'pfne' (make sure a superuser account already exists on the Juniper device - in this case the 'lab' user)

```
pfne@ubuntu1804-pfne:~$ ssh-copy-net 10.154.0.201 juniper
Username: lab
Password: lab123
All Done!
pfne@ubuntu1804-pfne:~$
```

##### [TMUX](https://linuxize.com/post/getting-started-with-tmux/) - multi window access from EVE-NG SSH session

```
Ctrl+b c Create a new window (with shell)

Ctrl+b % Split current pane horizontally into two panes
Ctrl+b " Split current pane vertically into two panes

Ctrl+b o Go to the next pane
Ctrl+b x Close the current pane

Ctrl+b ; Toggle between current and previous pane
Ctrl+b w Choose window from a list
Ctrl+b 0 Switch to window 0 (by number )
Ctrl+b , Rename the current window
```

#### Ansible

All tools should be setup for Ansible on the Linux host

Lets check netconf to R1 (this assumes SSH RSA key access is working - see above)

```
pfne@ubuntu1804-pfne:~$ ssh -s pfne@10.154.0.201 netconf
```

```
<!-- No zombies were killed during the creation of this user interface -->
<!-- user pfne, class j-super-user -->
<hello xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <capabilities>
    <capability>urn:ietf:params:netconf:base:1.0</capability>
    <capability>urn:ietf:params:netconf:capability:candidate:1.0</capability>
    <capability>urn:ietf:params:netconf:capability:confirmed-commit:1.0</capability>
    <capability>urn:ietf:params:netconf:capability:validate:1.0</capability>
    <capability>urn:ietf:params:netconf:capability:url:1.0?scheme=http,ftp,file</capability>
    <capability>urn:ietf:params:xml:ns:netconf:base:1.0</capability>
    <capability>urn:ietf:params:xml:ns:netconf:capability:candidate:1.0</capability>
    <capability>urn:ietf:params:xml:ns:netconf:capability:confirmed-commit:1.0</capability>
    <capability>urn:ietf:params:xml:ns:netconf:capability:validate:1.0</capability>
    <capability>urn:ietf:params:xml:ns:netconf:capability:url:1.0?protocol=http,ftp,file</capability>
    <capability>urn:ietf:params:xml:ns:yang:ietf-netconf-monitoring</capability>
    <capability>http://xml.juniper.net/netconf/junos/1.0</capability>
    <capability>http://xml.juniper.net/dmi/system/1.0</capability>
  </capabilities>
  <session-id>5363</session-id>
</hello>
```

Update the /etc/hosts file for name resolution

Update the /etc/ansible/hosts for Ansible inventory use

```
pfne@ubuntu1804-pfne:~$ cat /etc/ansible/hosts
# This is the default ansible 'hosts' file.
#
# It should live in /etc/ansible/hosts
#
#   - Comments begin with the '#' character
#   - Blank lines are ignored
#   - Groups of hosts are delimited by [header] elements
#   - You can enter hostnames or ip addresses
#   - A hostname/ip can be a member of multiple groups

[MX]
r1 ansible_connection=local
r2 ansible_connection=local
r3 ansible_connection=local
r4 ansible_connection=local
r5 ansible_connection=local
r6 ansible_connection=local
r7 ansible_connection=local
r8 ansible_connection=local
r9 ansible_connection=local

[SRX]
ce1 ansible_connection=local
ce2 ansible_connection=local
ce3 ansible_connection=local
ce4 ansible_connection=local
ce5 ansible_connection=local
ce6 ansible_connection=local

[JUNIPER:children]
MX
SRX

[JUNIPER:vars]
juniper_user=pfne
os=junos
```

Example file structure

```
pfne@ubuntu1804-pfne:~$ tree
.
├── ansible-automation
│   └── juniper_core
│       ├── conf
│       ├── group_vars
│       ├── host_vars
│       ├── info
│       │   └── interfaces
│       ├── logs
│       └── playbooks
```

Example playbook to pull configs and interface information and write to a file

```
pfne@ubuntu1804-pfne:~$ cat ansible-automation/juniper_core/playbooks/get_conf_and_int.yml
---
- name: GET
  hosts: JUNIPER
  roles:
  - Juniper.junos
  connection: local
  gather_facts: no

  # Execute tasks (plays) this way "ansible-playbook <path>/GET.yml --tags <tag-name>"
  tasks:

### JUNOS_GET_CONFIG SECTION
  # Execute "show configuration" and save output to a file
  - name: Pull Down The Configs
    juniper_junos_command:
      commands:
          - "show configuration | display set"
      host: "{{ inventory_hostname }}"
      logfile: ../logs/get_config.log
      format: text
      dest: "../conf/{{ inventory_hostname }}.conf"

### JUNOS_CLI SECTION
  # Get list of interfaces and save output to a file
  - name: GET-INTERFACES
    juniper_junos_command:
      commands:
          - "show interfaces terse"
          - "show interfaces descriptions"
      host: "{{ inventory_hostname }}"
      logfile: ../logs/get_interfaces.log
      format: text
      dest: "../info/interfaces/{{ inventory_hostname }}_get-interfaces.output"

### EOF ###
```

Example of output when running the playbook - using the `--limit=MX` switch to only run against the vMX routers

```
pfne@ubuntu1804-pfne:~$ ansible-playbook ansible-automation/juniper_core/playbooks/get_conf_and_int.yml --limit=MX

PLAY [GET] *******************************************************************************************************************************************************************************

TASK [Pull Down The Configs] *************************************************************************************************************************************************************
ok: [r5]
ok: [r2]
ok: [r1]
ok: [r3]
ok: [r4]
ok: [r7]
ok: [r9]
ok: [r8]
ok: [r6]

TASK [GET-INTERFACES] ********************************************************************************************************************************************************************
ok: [r1]
ok: [r2]
ok: [r5]
ok: [r3]
ok: [r4]
ok: [r8]
ok: [r6]
ok: [r7]
ok: [r9]

PLAY RECAP *******************************************************************************************************************************************************************************
r1                         : ok=2    changed=0    unreachable=0    failed=0
r2                         : ok=2    changed=0    unreachable=0    failed=0
r3                         : ok=2    changed=0    unreachable=0    failed=0
r4                         : ok=2    changed=0    unreachable=0    failed=0
r5                         : ok=2    changed=0    unreachable=0    failed=0
r6                         : ok=2    changed=0    unreachable=0    failed=0
r7                         : ok=2    changed=0    unreachable=0    failed=0
r8                         : ok=2    changed=0    unreachable=0    failed=0
r9                         : ok=2    changed=0    unreachable=0    failed=0
```

And the directory structure with the new files created

```
pfne@ubuntu1804-pfne:~$ tree
.
├── ansible-automation
│   └── juniper_core
│       ├── conf
│       │   ├── r1.conf
│       │   ├── r2.conf
│       │   ├── r3.conf
│       │   ├── r4.conf
│       │   ├── r5.conf
│       │   ├── r6.conf
│       │   ├── r7.conf
│       │   ├── r8.conf
│       │   └── r9.conf
│       ├── group_vars
│       ├── host_vars
│       ├── info
│       │   └── interfaces
│       │       ├── r1_get-interfaces.output
│       │       ├── r2_get-interfaces.output
│       │       ├── r3_get-interfaces.output
│       │       ├── r4_get-interfaces.output
│       │       ├── r5_get-interfaces.output
│       │       ├── r6_get-interfaces.output
│       │       ├── r7_get-interfaces.output
│       │       ├── r8_get-interfaces.output
│       │       └── r9_get-interfaces.output
│       ├── logs
│       │   ├── get_config.log
│       │   └── get_interfaces.log
│       └── playbooks
│           ├── get_conf_and_int.retry
│           └── get_conf_and_int.yml
```


#### [Salt](https://www.saltstack.com/solutions/netops/) Setup

Add Salt master to the Linux automation host

````
curl -o bootstrap_salt.sh -L https://bootstrap.saltstack.com
sudo sh bootstrap_salt.sh -M
````

Check version

```
pfne@ubuntu1804-pfne:~$ salt --version
salt 2018.3.3 (Oxygen)
```

Add a Salt minion - I used a CumulusVX device - with appropriate RAM - each proxy minion that will run on the minion will need 100M RAM - you need one proxy minion per Juniper router.

Add the follwing sources to the `/etc/apt/sources.list` file so the Salt minion will install 

````
deb http://ftp.us.debian.org/debian/ jessie main contrib non-free
````

Make sure you re-run `sudo apt update` and `sudo apt upgrade` after adding the sources

And then install the Salt minion (same script but without the `-M` switch)

````
curl -o bootstrap_salt.sh -L https://bootstrap.saltstack.com
sudo sh bootstrap_salt.sh
````
Check version

```
cumulus@cumulus:~$ salt-minion --version
salt-minion 2018.3.3 (Oxygen)
```

#### Adding an extra Linux host for docker

Add the same Ubuntu 18.04 Linux image used above and configure (by editing netplan.yml) with a new IP as before

Follow this [guide to install docker](https://linuxize.com/post/how-to-install-and-use-docker-on-ubuntu-18-04/)

Follow this [guide to install docker compose](https://linuxize.com/post/how-to-install-and-use-docker-compose-on-ubuntu-18-04/) - this allows you to define and manage multi-container Docker applications

Example [docker-compose](https://github.com/geerlingguy/awx-container/blob/master/docker-compose.yml) file for Ansible AWX (open source version of Ansible Tower)

Example [docker-compose](https://github.com/netbox-community/netbox-docker) file for NetBox (open-source IPAM/DCIM application)

#### Other Linux notes

VNC is used to remote to the host so adding a desktop environment is ideal if wanting to access web guis of docker applications (like NetBox etc.)

To install XFCE
```
sudo apt-get install xubuntu-desktop
```
To increase resolution 
```
sudo nano /etc/default/grub
```
Comment out the GRUB_GFXMODE section and change the resolution to something suitable (1280x960 as a starter) then
```
sudo update-grub
```

