# How to create a Tunnel IP

---

Link direct to the Oracle documentation.
- <a href="http://docs.oracle.com/cd/E19253-01/816-5166/6mbb1kq31/" target="_blank">`http://docs.oracle.com/cd/E19253-01/816-5166/6mbb1kq31/`</a> 

---

Specification for this examples.
- Network to reach on another machine: 10.137.0.0/17
- Another side IP tunnel: 192.168.70.1/30
- Your side IP tunnel: 192.168.70.2/30
- IP from another side: 192.159.205.12
- IP from your side: 192.234.0.23

---

Create a tunnel interface.
```console
-bash-3.2# ifconfig ip.tun0 plumb 
```

---

Command syntax.
```console
-bash-3.2# ifconfig ip.tun0 <IP_TUNNEL_SRC> <IP_TUNNEL_DST> tsrc <IP_INTERFACE_SRC> tdst <IP_INTERFACE_DST> up
```

Understanding the options.
- <IP_TUNNEL_SRC> - Non-routing IP of the source tunnel
- <IP_TUNNEL_DST> - Non-routing IP of the destination tunnel
- <IP_INTERFACE_SRC> - IP from source interface which we use to encapsulate the traffic
- <IP_INTERFACE_DST> - IP from destination interface which we use to unencapsulate the traffic

---
 
Add a route.
```console
-bash-3.2# route add <NETWORK_ON_TUNNER> <IP_TUNNEL_DST>
```

Understanding the options.
- <NETWORK_ON_TUNNER> - IP address of the network you want to access through the tunnel
- <IP_TUNNEL_DST> - Non-routing IP of the destination tunnel

---

Add network to the firewall.
```console
pass in from <IP_INTERFACE_DST> to any
pass out from any to <IP_INTERFACE_DST>
```

Firewall.
```console
pass in from 192.159.205.12 to any
pass out from any to 192.159.205.12
```

Understanding the options.
- <IP_INTERFACE_DST> - IP from destination interface which we use to unencapsulate the traffic

---

Commands.
```console
-bash-3.2# ifconfig ip.tun0 192.168.70.2 192.168.70.1 tsrc 192.234.0.23 tdst 192.159.205.12 up
-bash-3.2# route add 10.137.0.0/17 192.168.70.1
```