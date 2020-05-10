---
layout: post
title: "The Docker Macvlan Network"
date: 2020-05-10
image: '/images/thumbs/thumb_docker_macvlan_network.png'
description:
category: ''
tags:
twitter_text:
introduction: It's not so simple afterall!
published: true
---

The docker macvlan network - documented [here](https://docs.docker.com/network/macvlan/) - is probably the least explored docker network.
Engineers venturing towards the macvlan network are probably trying to fill a very unique requirement which the other types of network are not able to satisy.

In short, they are already frustrated.

In this post, I try to provide all the steps required to setup a docker macvlan network, so that people like me can try this out quickly and verify if it suits their needs.

**Step1**: Create a linux VM. Linux lite is good. Setup the network - be sure to select **PROMISCUOUS MODE** in network settings.
![_config.yml]({{ site.baseurl }}/images/macvlan_virtualbox_promiscuous_mode.png)

On VirtualBox 6.1.6 r137129, I configured the following:
+ eth0: Network Type: Bridged Host, Promiscuous Mode set to "Deny"
+ eth1: Network Type: NATNetwork, Promiscuous Mode set to "Allow All"

PROMISCUOUS MODE on the eth0 did not work for me with my WiFi Router. I am not sure if it was because of the VirtualBox configuration or if my WiFi Router was blocking it.

Also make sure that PROMISCUOUS MODE is enabled only on one of the interfaces. When I had it enabled on both the interfaces, it stopped working on eth1 as well.

**Step2**: Create a docker macvlan network. 

Pay attention to the IP subnet that is chosen here.
Find the route settings for eth1 on your machine. We will use this info to create the macvlan network.
````bash
linux1# ip route | grep eth1
default via 10.0.2.1 dev eth1 proto dhcp metric 101 
10.0.2.0/24 dev eth1 proto kernel scope link src 10.0.2.4 metric 101
````

Run the docker command to create the macvlan network attached to eth1. We will use the values from the above output, for **subnet, gateway and parent**.
````bash
linux1# docker network create -d macvlan --subnet=10.0.2.0/24 --gateway=10.0.2.1 -o parent=eth1 mac1
979502ad94bd98cb5a7d225285acadd4c0515f10cb2d6fc38d8abb71d1779ef2
````

**Step3**: Create a docker container attached to the macvlan network. Assign an unused IP from the subnet.
Docker command to create the container on mac1:
````bash
linux1# docker run -d -p 5000:5000 --name mac1 --network mac1 --ip 10.0.2.5 vimal1984/tester
c2af4dbe07ce11028ad01adfb3bb6ec679dd828e11c8df3ee65bc62e9d907fcd
````

**Step4**: Test

At this point, we should be able to communicate with the external world through the gateway.
In the VirtualBox NatNetwork, I have configured NAT as **192.168.1.12:5000 <-> 10.0.2.5:5000**.
192.168.1.12 is the IP address of the VirtualBox Host, and 10.0.2.5 is the IP of the container.

If you used the same docker image as shown above, you can access a simple UI using the IP **192.168.1.12:5000**, which shows the container ID and its IP address.
![_config.yml]({{ site.baseurl }}/images/macvlan_tester_container_ui.png)

The following is the tcpdump on eth1 of the Docker Host, when I access the UI.

````bash
linux1# tcpdump -n -i eth1
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth1, link-type EN10MB (Ethernet), capture size 262144 bytes
18:24:38.083836 IP 192.168.1.12.58735 > 10.0.2.5.5000: Flags [S], seq 26679, win 32768, options [mss 1460], length 0
18:24:38.083905 ARP, Request who-has 10.0.2.1 tell 10.0.2.5, length 28
18:24:38.084424 ARP, Reply 10.0.2.1 is-at 52:54:00:12:35:00, length 46
18:24:38.084434 IP 10.0.2.5.5000 > 192.168.1.12.58735: Flags [S.], seq 568848428, ack 26680, win 64240, options [mss 1460], length 0
18:24:38.084599 IP 192.168.1.12.58735 > 10.0.2.5.5000: Flags [.], ack 1, win 32768, length 0
18:24:38.084701 IP 192.168.1.12.58735 > 10.0.2.5.5000: Flags [P.], seq 1:339, ack 1, win 32768, length 338
18:24:38.084713 IP 10.0.2.5.5000 > 192.168.1.12.58735: Flags [.], ack 339, win 63902, length 0
18:24:38.132492 IP 10.0.2.5.5000 > 192.168.1.12.58735: Flags [P.], seq 1:18, ack 339, win 63902, length 17
18:24:38.132971 IP 10.0.2.5.5000 > 192.168.1.12.58735: Flags [FP.], seq 18:901, ack 339, win 63902, length 883
18:24:38.133251 IP 192.168.1.12.58735 > 10.0.2.5.5000: Flags [.], ack 902, win 31867, length 0
18:24:38.133630 IP 192.168.1.12.58735 > 10.0.2.5.5000: Flags [F.], seq 339, ack 902, win 31867, length 0
18:24:38.133653 IP 10.0.2.5.5000 > 192.168.1.12.58735: Flags [.], ack 340, win 63902, length 0
````
**Step5**: Enable communication between container and host.

By default, the docker container on mac0 will not be able to communicate with the docker host. This is by design with macvlan networks.

````bash
c2af4dbe07ce:/ # ping -c 1 10.0.2.4
PING 10.0.2.4 (10.0.2.4) 56(84) bytes of data.
From 10.0.2.5 icmp_seq=1 Destination Host Unreachable

--- 10.0.2.4 ping statistics ---
1 packets transmitted, 0 received, +1 errors, 100% packet loss, time 0ms
````

In order to establish communication between the container and the host, we will need to add another interface on the host and configure it correctly. This will also require another IP from the macvlan subnet to be used.

````bash
ip link add mac1_eth1 link eth1 type macvlan mode bridge
ip addr add 10.0.2.6/24 dev mac1_eth1
ifconfig mac1_eth1 up

ip route | grep mac1_eth1
10.0.2.0/24 dev mac1_eth1 proto kernel scope link src 10.0.2.6
````

After this, the container can ping the host.

````bash
c2af4dbe07ce:/ # ping -c 1 10.0.2.4
PING 10.0.2.4 (10.0.2.4) 56(84) bytes of data.
64 bytes from 10.0.2.4: icmp_seq=1 ttl=64 time=0.094 ms

--- 10.0.2.4 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.094/0.094/0.094/0.000 ms
````
