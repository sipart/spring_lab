* Azure setup for EVE-NG community edition

* Use a VM with 's_v3' in the name - no need to provision with premium disk support and do not worry about the size of the temp storage as you can expand this later (see instruction below)

![nv-azure_s_v3](/img/azure_s_v3.png)

* Set auth as a username and password of your choosing i.e. evengadmin
    * Setup FW rule
        * Source IP = HOME DSL PUBLIC IP
        * Source port = any
        * Destination IP = any
        * Destination Port Ranges = <22,80>
        * Name = port_22_80_inbound
* Setup DNS name i.e. eveng (eveng.ukwest.cloudapp.azure.com)

* Boot up VM and SSH (via portal SSH) to DNS name and login using your auth credentials
* Set root password
```
evengadmin@eve-ng:~$ sudo passwd root
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

* Set abiity to login as root
```
sudo nano /etc/ssh/sshd_config
```
Change:
```
PermitRootLogin yes

PasswordAuthentication yes
```

* Change interface name to "eth0" (if needed!)
```
nano /etc/udev/rules.d/70-persistent-net.rules
```
Update/Upgrade and Reboot
```
sudo apt update

audo apt upgrade

sudo shutdown -r now
```
* Logon via desktop SSH client as root and install eve-ng
```
wget -O - http://www.eve-ng.net/repo/install-eve.sh | bash -i 
```
Reboot
```
sudo shutdown -r now
```

* You will get prompts for root password and other settings which you can just accept and then VM will reboot again


* Update grub to set this default
```
sed -i -e  's/GRUB_CMDLINE_LINUX_DEFAULT=.*/GRUB_CMDLINE_LINUX_DEFAULT="net.ifnames=0 noquiet"/' /etc/default/grub

update-grub
```
Reboot
```
sudo shutdown -r now
```


* How to re-size the temp storage disk you get in Azure (EVE-NG only uses the first disk so even though the size of VM I used has an additional 315G disk it is no use for EVE-NG)
https://docs.microsoft.com/en-us/azure/virtual-machines/linux/expand-disks

Power the VM down and copy the following into the VMs cloud shell to get the current size and disk name - your resource-group may be different:
```
az disk list \
>     --resource-group cloud-shell-storage-westeurope \
>     --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
>     --output table
```
Ouput:
```
Name                                             Gb
-----------------------------------------------  ----
eveng_OsDisk_1_882d1ef3e665477b879435fcc62a8972  30
```
Copy the following into the VMs cloud shell to re-size the disk:
```
az disk update \
    --resource-group cloud-shell-storage-westeurope \
    --name eveng_OsDisk_1_882d1ef3e665477b879435fcc62a8972 \
    --size-gb 100
```
Re-start you VM

You are now ready to browse to your EVE-NG and check all is OK - and transfer images - check out the [EVE-NG cookbook](https://www.eve-ng.net/images/EVE-COOK-BOOK-1.8.pdf) for guidance
