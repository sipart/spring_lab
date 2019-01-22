# MPLS Segment Routing Core for a [EVE-NG](http://www.eve-ng.net/) virtual lab

Lab created from the example in the Juniper Day One book [Configuring Segment Routing with Junos](https://www.juniper.net/uk/en/training/jnbooks/day-one/configuring-segment-routing-junos/index.page) by [Julian Lucek](https://twitter.com/julianlucek?lang=en) and [Krzysztof Szarkowicz](https://www.oreilly.com/pub/au/6140)

A set of initial lab configs for a post that will appear on [Tech Snippets](https://sipart.github.io/) in the future (to include detail on using EVE-NG on the Google Compute Platform as well!)

* MPLS core is using ISIS as the IGP
* PE router images are all vMX QEMU Junos 17.3R3-S2.2
* R4 and R7 are the route reflectors
* CE vSRX site routers will be Junos v12.1x47-d15.4 QEMU images
* root user password is lab123
* lab user password is lab123
* Initial folder holds the configs for the devices and also an export from EVE-NG of the initial setup (upto page 15 of the book)

![jdo-spring](/img/SPRING-lab.png)
