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

# Solaris 10 - Install using JUMPSTART

---

**Requiriments**
- Solaris 10 image ISO

---

We will need:
* Another machine with Solaris. I am going to use a VirtualBox machine with Solaris as I don't have another Solaris machine. 
* The ethernet should be configured as "bridged" in VirtualBox and the host machine must be in the same subnet as the machine where we want to install Solaris.
* The Solaris 10 64bits install DVD for SPARC (sun4v). I downloaded the image and mounted it inside VirtualBox.
* The Solaris server (VirtualBox) and the machine to install must be in the same subnet.


We load OpenSolaris in the virtual machine and we mount the install DVD:
1.	Mount the DVD
2.	Be root
3.	mkdir -p /path/to/anywhere/sol
4.	mkdir -p /path/to/anywhere/config
5.	cd /cdrom/cdrom0/S0/Solaris_10/Tools
6.	./setup_install_server /path/to/anywhere
7.	add in /etc/dfs/dfstab: share -F nfs -o ro,anon=0 -d "install dir" /path/to/anywhere/sol and add the path to config too.
8.	Run shareall and share and check the folders are shared.
9.	Check that nfsd is running: svcs -l svc:/network/nfs/server:default if not, execute it svcadm enable svc:/network/nfs/server:default
10.	cd /cdrom/cdrom0/S0/Solaris_10/Tools
11.	./setup_install_server -b /path/to/boot
12.	cp /cdrom/cdrom0/s0/Solaris_10/Misc/jumpstart_sample/* /path/to/config
13.	edit /path/to/config/rules
14.	write: network net_ip.0 && arch sparc – profile -
15.	edit profile
16.	write install_type initial_install / system_type standalone
17.	edit sysidcfg
18.	add standard lines without root_passwd
19.	cd /
20.	umount the DVD
21.	Edit /etc/hosts and add a new entry with host1 ip_t1000
22.	cd /path/to/anywhere/Solaris_10/Tools
23.	./add_install_client -e ethernet_t1000 -s ip_server:/path/to/anywhere/sol -c ip_server:/path/to/anywhere/config -p ip_server:/path/to/anywhere/config host1 sun4v
24.	Check in /etc/bootparams root_server is not localhost, it has to be the OpenSolaris server ip.
25.	Boot the T1000, when we see the promt >ok type boot net -v – install
26.	The installation will start, follow the onscreen instructions.
