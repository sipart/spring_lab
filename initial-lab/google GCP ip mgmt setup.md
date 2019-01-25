10.132.0.0/20 is the europe-west1 region VPC

# Cloud9 setup on EVE-NG server (allows Internet and mgmt)

    ip address add 10.132.0.10/20 dev pnet9

    echo 1 > /proc/sys/net/ipv4/ip_forward

    iptables -t nat -A POSTROUTING -o pnet0 -s 10.132.0.0/20 -j MASQUERADE

# To make pnet9 IP persistent

1- Edit the interfaces file

    sudo nano /etc/network/interfaces

2- edit eth9 for your preferred IP

    iface eth9 inet manual
    auto pnet9
    iface pnet9 inet static
        address 10.132.0.10
        netmask 255.255.240.0
        bridge_ports eth9
        bridge_stp off

# To make iptables persistent

1-

    sudo apt-get install iptables-persistent

2-

    sudo netfilter-persistent save

3-

    sudo netfilter-persistent reload

4- reboot the system and do the following command:

    sudo iptables -t nat -L

it should show the rule as persistent

# For the /proc/sys/net/ipv4/ip_forward to be persistent across reboots

1-

    sudo nano /etc/sysctl.conf

2- Uncomment 'net.ipv4.ip_forward=1' and save

3- issue the following command:

    sudo sysctl -p /etc/sysctl.conf

4- reboot the system and do the following command:

    cat /proc/sys/net/ipv4/ip_forward

it should show 1 in the output

# Setup of network device mgmt. and Linux box mgmt.

Connect devices mgmt. ints (fxp0 on vMX) to cloud9 network object

Set IP on mgmt. interfaces -

10.132.0.201/20 and up for PEs

10.132.0.221/20 and up for CEs

# Add automation Linux box image ([18.04 Ubuntu server](https://ipnet.xyz/2018/06/ubuntu-image-for-eve-ng-python-for-network-engineers/)) to EVE-NG and add to any topology

** Edit netplan file to get Linux box networked

    sudo nano /etc/netplan/01-netcfg.yaml

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

Check if you can ping 8.8.8.8 :-)

Extras to install on the Linux box:

        sudo ansible-galaxy install Juniper.junos

        sudo pip2 install git+https://github.com/Juniper/py-junos-eznc.git

        sudo pip install git+https://github.com/networkop/ssh-copy-net.git

Create SSH key when logged in as pfne:

        ssh-keygen -t rsa -b 2048

Use SSH copy util to copy SSH key to Juniper boxes (make sure pfne superuser already exists on the Juniper device with password of pfne123)

    pfne@ubuntu1804-pfne:~$ ssh-copy-net 10.132.0.201 juniper
    Username: pfne
    Password: pfne123
    All Done!
    pfne@ubuntu1804-pfne:~$

# [TMUX](https://linuxize.com/post/getting-started-with-tmux/) - multi window access from EVE-NG SSH session - 
    Ctrl+b c Create a new window (with shell)

    Ctrl+b % Split current pane horizontally into two panes
    Ctrl+b " Split current pane vertically into two panes

    Ctrl+b o Go to the next pane
    Ctrl+b x Close the current pane

    Ctrl+b ; Toggle between current and previous pane
    Ctrl+b w Choose window from a list
    Ctrl+b 0 Switch to window 0 (by number )
    Ctrl+b , Rename the current window

# Ansible

All tools should be setup for Ansible on the Linux host

Lets check netconf to R1 (this assumes SSH RSA key access is working - see above)

    pfne@ubuntu1804-pfne:~$ ssh -s pfne@10.132.0.201 netconf
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
