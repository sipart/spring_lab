# MPLS Segment Routing Core for a [EVE-NG](http://www.eve-ng.net/) virtual lab

Lab created from the example in the Juniper Day One book [Configuring Segment Routing with Junos](https://www.juniper.net/uk/en/training/jnbooks/day-one/configuring-segment-routing-junos/index.page) by [Julian Lucek](https://twitter.com/julianlucek?lang=en) and [Krzysztof Szarkowicz](https://www.oreilly.com/pub/au/6140)

A set of initial lab configs for a post that will appear on [Tech Snippets](https://sipart.github.io/) in the future (to include detail on using EVE-NG on the Google Compute Platform as well!)

At this time the EVE-NG server is in the Google Compute Platform (24 vCPUs, 90 GB memory for this lab) as you get $300 worth of credit you can use as you please for 12 months. Azure gives you $250 but limited machine types and only for the first 30 days and Amazon doesn't have machines that support nested virtulization - so no good for EVE-NG.

* MPLS core is using ISIS as the IGP
* PE router images are all vMX QEMU Junos 17.3R3-S2.2
* R4 and R7 are the route reflectors
* CE vSRX site routers will be Junos v12.1x47-d15.4 QEMU images
* Juniper root user password is lab123
* Juniper lab user password is lab123
* Linux host root user password is root
* Linux host pfne user password is pfne
* The pfne user has a RSA key to communicate with the network devices
* Initial folder holds the configs for the devices and also exports from EVE-NG of the initial setup (upto page 15 of the book)

![jdo-spring](/img/SPRING-lab.png)
