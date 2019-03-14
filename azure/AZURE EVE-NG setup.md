* Azure setup

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

```
simon@Azure:~$ az disk list \
>     --resource-group cloud-shell-storage-westeurope \
>     --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
>     --output table
```
```
Name                                             Gb
-----------------------------------------------  ----
eveng_OsDisk_1_882d1ef3e665477b879435fcc62a8972  30
```
```
az disk update \
    --resource-group cloud-shell-storage-westeurope \
    --name eveng_OsDisk_1_882d1ef3e665477b879435fcc62a8972 \
    --size-gb 100
```

** ```DO NOT DO THE BELOW IN AZURE - LEAVE THE KERNEL IN THE VM USING THE AZURE ONE ``` **

* Move of GCP or Azure kernel - DOES NOT WORK (works in GCP) - makes VM unbootable - info' for future reference if I can find out how to get this working!!

Check kernel in use
```
uname -a

Linux eve-ng 4.15.0-1037-azure #39~16.04.1-Ubuntu SMP Tue Jan 15 17:20:47 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
```
Create new directory `/boot` and move the 'cloud' image kernels then update grub 
```
root@eve-ng:~# cd /boot
root@eve-ng:/boot# mkdir ./old/
```
```
root@eve-ng:/boot# ls
config-4.15.0-1037-azure          old
config-4.9.40-eve-ng-ukms-2+      System.map-4.15.0-1037-azure
grub                              System.map-4.9.40-eve-ng-ukms-2+
initrd.img-4.15.0-1037-azure      vmlinuz-4.15.0-1037-azure
initrd.img-4.9.40-eve-ng-ukms-2+  vmlinuz-4.9.40-eve-ng-ukms-2+
```
```
root@eve-ng:/boot# mv *4.15* ./old/
```

```
update-grub

Generating grub configuration file ...
Found linux image: /boot/vmlinuz-4.9.40-eve-ng-ukms-2+
Found initrd image: /boot/initrd.img-4.9.40-eve-ng-ukms-2+
done
```
Reboot
```
shutdown -r now
```
Check kernel in use
```
uname -a

Linux sipart-eve 4.9.40-eve-ng-ukms-2+ #4 SMP Fri Sep 15 02:07:02 CEST 2017 x86_64 x86_64 x86_64 GNU/Linux
```

* Doing the above in Azure will render the VM unbootable - see below to get the VM bootable and reverse the kernel move

Go into serial console after starting the VM

Stab the ESC key until you get the grub menu (you may have to reboot the VM to catch the grub menu)

Go into advanced options and press 'e' to edit the grub file and alter the paths to the 'old' kernel and initrd files.

Once booted you can login, move the files back and update-grub - phewww!
