# MPLS Segment Routing Core for a [EVE-NG](http://www.eve-ng.net/) virtual lab

Lab created from the example in the Juniper Day One book [Configuring Segment Routing with Junos](https://www.juniper.net/uk/en/training/jnbooks/day-one/configuring-segment-routing-junos/index.page){:target="_blank"} by [Julian Lucek](https://twitter.com/julianlucek?lang=en){:target="_blank"} and [Krzysztof Szarkowicz](https://www.oreilly.com/pub/au/6140){:target="_blank"}

A set of initial lab configs for this post: WIP (to include detail on using EVE-NG on the Google Compute Platform)

* MPLS core is using ISIS as the IGP
* vMX images are all QEMU Junos 17.3R3-S2.2
* vMX4 and 7 are the route reflectors
* vSRX site routers will be Junos v12.1x47-d15.4 QEMU images (running DHCP in the instance for the site PCs)
