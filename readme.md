# MPLS Segment Routing Core for a [EVE-NG](http://www.eve-ng.net/) virtual lab

Lab created from the example in the Juniper Day One book [Configuring Segment Routing with Junos](https://www.juniper.net/uk/en/training/jnbooks/day-one/configuring-segment-routing-junos/index.page) by [Julian Lucek](https://twitter.com/julianlucek?lang=en) and [Krzysztof Szarkowicz](https://www.oreilly.com/pub/au/6140).

Also setup [Forward Essentials](https://www.forwardnetworks.com/network-mapping-software/) (as it's free) to crawl and map the lab.

Now with added [Salt!](https://www.saltstack.com/solutions/netops/) helped again by another [Juniper Day One book authored by Peter Klimai](https://github.com/pklimai/day-one-junos-salt) and [Mirceau Linic's](https://twitter.com/mirceaulinic) excellent Salt and automation resouces via his [site](https://mirceaulinic.net/) and [github](https://github.com/mirceaulinic) page.

A set of initial lab configs is included in this repo for a post that will hopefully appear on [Tech Snippets](https://sipart.github.io/) in the future (to include detail on using EVE-NG on the Google Compute Platform as well!).

At this time the EVE-NG server is in the Google Compute Platform (24 vCPUs, 90 GB memory for this lab) as you get $300 worth of credit you can use as you please for 12 months. Azure gives you $250 but limited machine types and only for the first 30 days and Amazon doesn't have machines that support nested virtulization - so no good for EVE-NG.

* MPLS core is using ISIS as the IGP
* PE router images are all vMX QEMU Junos 17.3R3-S2.2
* R4 and R7 are the route reflectors
* CE vSRX site routers will be Junos v12.1x47-d15.4 QEMU images
* Juniper root user password is lab123
* Juniper lab user password is lab123
* Linux automation host root user password is root
* Linux automation host pfne user password is pfne
* Ansible has been used and I have created some low level [playbooks](https://github.com/sipart/spring_lab/tree/master/playbooks) to gather some information
* CumulusVX (Salt minion) cumulus user password is CumulusLinux!
* The pfne user has a local public SSH key to communicate with the network devices from the Linux automation server
* Initial folder holds the configs for the devices and also exports from EVE-NG of the initial setup (upto page 15 of the book)
* More comprehensive information on how to create the lab and other components [here](https://github.com/sipart/spring_lab/blob/master/GCP%20setup%20and%20more.md)

EVE-NG lab topology:

![jdo-spring](/img/SPRING-lab1.png)

EVE-NG lab topology in Forward Essentials (collector installed on the Linux automation box):

![fwd-essentials](/img/springcoreinFWDess1.png)
