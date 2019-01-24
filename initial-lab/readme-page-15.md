# This set of initial labs will get you to page 15 in the Juniper Segment Routing Day One book.

* Also included is an EVE-NG lab export of this initial setup (no CE configs - but they appear in the topology - check newer uploads/export dates for updates) - the toplogy also includes a Linux box (18.04 Ubuntu server) that has automation tools installed - from [here](https://ipnet.xyz/2018/06/ubuntu-image-for-eve-ng-python-for-network-engineers/). Be aware Ubuntu 18.04 server uses netplan for IP configuration - don't you love Linux!!

* The syslog filtering is used to remove some annoying messages from the virtual console connections used by EVE-NG when you don't have a mgmt. connection - these can be excluded if you configure your mgmt. in-between the stream of messages!

* The IPv6 ISIS adjacencies are created using the automatic link local addressing - so no need to address them individually.

* In my case and at this time the EVE-NG server is in the Google Compute Platform (24 vCPUs, 90 GB memory for this lab) as you get $300 worth of credit you can use as you please for 12 months. Azure gives you $250 but limited machine types and only for the first 30 days and Amazon doesn't have machines that support nested virtulization - so no good for EVE-NG.

* The mgmt. IP addressing is aligned with the Google Compute VPC subnet (10.132.0.0/20 is the europe-west1 region VPC in this case). Adjust the routing on the EVE-NG server to get SSH reachability from it to all the devices - 'ip route add 10.132.0.0/20 dev pnet0')
