---
title: "Solaris 10 - Procedimento básico de instalação"
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
  - solaris-10
tags:
  - solaris-10
  - sparc
  - sysadmin
last_modified_at: 2021-04-22T23:00:00-03:00
---

# Solaris Firmware Update procedures via the "sysfwdownload"

---

**PREREQUISITES/NOTES ( Oracle )**

1.  The sysfwdownload utility must be available and executable on the host.
    Oracle includes the latest sysfwdownload utility with each MyOracle
    Support (MOS) patch for Sun System Firmware.  You are encouraged to use
    the version included in the patch by default vs older versions from prior
    patches or versions already installed on your system.

2.  Access to the documentation for "Oracle Integrated Lights Out Manager (ILOM)
    may be required for additional setup procedures.  This information can be
    accessed from http://www.oracle.com/technetwork/documentation/sys-mgmt-networking-190072.html

3.  Updating the Sun System Firmware requires a host (Solaris) outage.  It is
    highly recommended that you review all of the prerequisites and procedures
    prior to performing the task to avoid unscheduled outages.

4.  To identify the version of Sun System Firmware currently on your system,
    use the "sysfwdownload -g" command option.

---

Download the firmware.
- <a href="https://www.oracle.com/servers/technologies/firmware/release-history-jsp.html" target="_blank">`https://www.oracle.com/servers/technologies/firmware/release-history-jsp.html`</a> 

---

First verify the SunSystem Firmware version.
```console
-bash-3.2# ./sysfwdownload -g
``` 

From a Solaris terminal window on the system to be upgraded, type the following. Remember our system will be rebooted, after the execution!
```console
-bash-3.2# ./sysfwdownload -u [image].pkg
```

Answer yes/no to the following question.
```console
WARNING: Host will be powered down for automatic firmware update when download is completed.
Do you want to continue(yes/no)?
```

Log back into the Solaris host, now run ./sysfwdownload -g and verify the SunSystem Firmware version is the one you loaded.
```console
-bash-3.2# ./sysfwdownload -g
```