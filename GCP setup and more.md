## Resources for setting up EVE-NG in the Google Compute Platform

The linked resources below are ideal for EVE-NG 'cloud' setup - further information in this document are related to the extra steps for my lab setup.

Comprehensive post by [ithitman](http://ithitman.blogspot.com/2018/04/configuring-eve-ng-on-google-compute.html)

Tony Es similar but basic blog [post](https://showipintbri.blogspot.com/2018/08/eve-ng-in-cloud.html) which points to his comprehensive video on [YouTube](https://www.youtube.com/watch?v=HDHsMgCs0XU) - NOTE: take specual attention to section at 28:34 about GCP kernel!

Watch alternative [Azure](https://youtu.be/hdUSNWMHbUU) setup by thelantamer

### Notes and steps for this lab setup

I setup my VM in the europe-west1 region due to the need for more cores not availiable in the UK DCs.

10.132.0.0/20 is the europe-west1 region VPC

#### Cloud9 setup on EVE-NG server - allows Internet access for Linux host in topology and EVE-NG/Linux host access to network device mgmt. - commands assume logged in as root.

```
ip address add 10.132.0.10/20 dev pnet9
echo 1 > /proc/sys/net/ipv4/ip_forward
iptables -t nat -A POSTROUTING -o pnet0 -s 10.132.0.0/20 -j MASQUERADE
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
        address 10.132.0.10
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
default         10.132.0.1      0.0.0.0         UG    0      0        0 pnet0
10.132.0.0      *               255.255.240.0   U     0      0        0 pnet9
10.132.0.1      *               255.255.255.255 UH    0      0        0 pnet0

root@sipart-eve:~# ip route list
default via 10.132.0.1 dev pnet0
10.132.0.0/20 dev pnet9  proto kernel  scope link  src 10.132.0.10
10.132.0.1 dev pnet0  scope link
```

#### Setup of network device mgmt. and Linux box mgmt.

Connect devices mgmt. ints (fxp0 on vMX) to cloud9 network object

Set IP on mgmt. interfaces -

10.132.0.201/20 and up for PEs

10.132.0.221/20 and up for CEs

#### Add automation Linux box image ([18.04 Ubuntu server](https://ipnet.xyz/2018/06/ubuntu-image-for-eve-ng-python-for-network-engineers/)) to EVE-NG and add to any topology - login as pfne.

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
     addresses: [10.132.0.200/20]
     gateway4: 10.132.0.2
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
pfne@ubuntu1804-pfne:~$ ssh-copy-net 10.132.0.201 juniper
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
pfne@ubuntu1804-pfne:~$ ssh -s pfne@10.132.0.201 netconf
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

#### Forward Essentials

Forward Essentials collects and organizes data about your network automatically producing an always up-to-date topology and inventory. Configuration and state data is searchable network-wide across all your devices.

After installation (you need to sign up [here](https://www.forwardnetworks.com/network-mapping-software/) to get a logon to the web app and download software - essentials is free) start the collector on the Linux automation host (it does not run as a service) to do device discovery and snapshots.

Installation on Linux

Open a terminal on the host machine and run this command. No need to use sudo. Your command will most likely differ due to the token.

```
bash <(curl  -s 'https://fwd.app/api/software/client/installScript?token=a3b6f690-92cb-4082-83af-a09f884b8d29') && export PATH=$HOME/.fwd/bin:$PATH
```

Path to Forward Networks collector software '$HOME/.fwd/bin/'

Open a new SSH session to the Linux host and start the collector (if the session or daemon is interrupted then the collector will stop):

```
$HOME/.fwd/bin/fwd daemon
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
