---
title: "Solaris 10 - Ajustando TIMEZONE"
classes: wide
header:
  overlay_image: /assets/images/solaris-optmize.jpg
  og_image: /assets/images/solaris-optmize.jpg
  teaser: /assets/images/solaris-optmize-thumb.jpg
  image_description: "Fundo azulado com desenho de meio sol"
  #caption: "Foto/Imagem: [**signorelli**](https://pixabay.com/)"
#  #actions:
#  #  - label: "Leia mais"
#  #    url: "https://cr-signorelli.github.io/"
categories:
  - network
tags:
  - network
  - documentação
last_modified_at: 2021-04-22T23:00:00-03:00
---

# How configure a network interface

---

**Enable the network card** 

Command ifconfig is used in Solaris to configure the network interfaces.
```console
-bash-3.2# ifconfig igb0 plumb
```

Show following type of output which means device is enabled and is ready to configure ip address and netmask.
```console
-bash-3.2# ifconfig -a
```

Expected outcome.
```
igb0: flags=842 mtu 1500
inet 0.0.0.0 netmask 0
ether 3:22:11:6d:2e:1f
```

---

**Configuring IP address**

Configuring ipaddress and netmask and making the interface status as up.
```console
-bash-3.2# ifconfig igb0 192.0.2.10 netmask 255.255.255.0 up
```

Will now show the ip address , netmask and up status as follows.
```console
-bash-3.2# ifconfig -a
```

Expected outcome.
```console
igb0: flags=843 mtu 1500
inet 192.0.2.10 netmask ffffff00 broadcast 192.9.2.255 ether 3:22:11:6d:2e:1f
```

---

**Configuring Virtual interface**

Virtual interface can be configured to enable igb0 reply to more then one ip addresses.
```console
-bash-3.2# ifconfig igb0:1 198.51.100.10 netmask 255.255.255.0 up
```

Will show the original igb0 and alias interface.
```console
-bash-3.2# ifconfig -a
```

Expected outcome.
```console
igb0: flags=843 mtu 1500
inet 192.0.2.10 netmask ffffff00 broadcast 192.9.2.255 ether 3:22:11:6d:2e:1f
igb0:1: flags=842 mtu 1500
inet 198.51.100.10 netmask ffffff00 broadcast 172.40.255.255
```

---

**Ip-forwarding**

IP forwarding allows you to forward all requests coming for a certain port or URL to be redirected to a specified IP address. 

IP forwarding is on by default but can be turned off by following command.
```console
-bash-3.2# ndd -set /dev/ip ip_forwarding 0
```

IP forwarding becomes enabled automatically when system detects more then one interface at the booting time. 
```
The file involved is /etc/rc2.d/S69inet
```

---

**Router Configuration**

To create a permanent route.
```console
-bash-3.2# route -p add default 192.0.2.2 1
```

To change a route.
```console
-bash-3.2# route change default 192.0.2.1 1
```

The system needs a default router which will allow the machine.
```console
-bash-3.2# route add -net 10.0.0.0 -netmask 255.0.0.0 198.51.100.1 1
```

---

**Network Terms CIDR**

CIDR : Classless Inter-Domain Routing – the notation often used instead of writing the subnet mask along with ip-address. It has network prefix at the end of a address as / number of network bits.This means that the IP address 192.200.20.10 with the subnet mask 255.255.255.0 can also be expressed as 192.200.20.10/24. The /24 indicates the network prefix length, which is equal to the number of continuous binary one-bits in the subnet mask (11111111.11111111.11111111.000000). Zeros are for addressing the hosts on this network.

```console
-bash-3.2# ndd -set /dev/hme adv_100fdx_cap 1
-bash-3.2# ndd -get /dev/dmfe0 link_mode
-bash-3.2# /usr/sbin/ndd /dev/dmfe0 link_mode
```

```console
-bash-3.2# set dmfe dmfe_adv_100fdx_cap=1
-bash-3.2# set dmfe:dmfe_adv_autoneg_cap=0
-bash-3.2# set dmfe:dmfe_adv_100fdx_cap=1
-bash-3.2# set dmfe:dmfe_adv_100hdx_cap=0
```


# How to enable interface IPv6 on Solaris10-SPARC

---

Server specification for this examples.
- bge1 = physical network interface 
- 2001:db8:1:0:0:0:0:100/128 = interface ip address 
- 2001:db8:1:0:0:0:0:1 = default gateway 

---

Enable the interface.
```console
-bash-3.2# ifconfig inet6 <interface> plumb up
```

Example.
```sh
-bash-3.2# ifconfig inet6 bge1 plumb up
```
---

Configure IP on interface.
```console
-bash-3.2# ifconfig <interface> inet6 <address_ipv6>/<netmask> up

```
Example.
```console
-bash-3.2# ifconfig bge1 inet6 addif 2001:db8:1:0:0:0:0:100/128 up
```

---

Adding a static route.
```console
-bash-3.2# route -p add -inet6 default <gateway_ipv6>
```

Example.
```console
-bash-3.2# route -p add -inet6 default 2001:db8:1:0:0:0:0:1
```

---

Adding the interface on system boot.
```console
-bash-3.2# echo "addif <address_ipv6>/<netmask> up" > /etc/hostname6.<interface>
```

Example.
```console
-bash-3.2# echo "addif 2001:db8:1:0:0:0:0:100/128 up" > /etc/hostname6.bge1
```

---

Adjust files permissions.
```console
-bash-3.2# chmod 644 /etc/hostname.<interface>
```

Example.
```console
-bash-3.2# chmod 644 /etc/hostname.bge1
```