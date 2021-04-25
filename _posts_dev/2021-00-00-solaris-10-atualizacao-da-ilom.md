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


# ILOM - configurando IP address na NET-MGT

---

Uma vez logado na console da ILOM use os comandos abaixo para configurar a rede.
```console
> cd /SP/network
> set pendingipaddress=192.0.2.10
> set pendingipnetmask=255.255.255.0 
> set pendingipgateway=192.0.2.1
> set commitpending=true
```

# ILOM - Troca de senha

---

Por padrão os servidores Oracle SPARC a ILOM vem com usuário e senha pré definidos.
* usuário: root
* senha: changeme

Uma vez logado na console da ILOM use o comando abaixo para trocar a senha de root.
```console
> set /SP/users/root senha_nova
```

# Update ILOM by CLI

---

**Requiriments**
- One computer connected in the same network and broadcast domain
- You need a web-server installed and enable
- Web-server is enabling on port 8888

---

Access the ILOM using console.
```console
-> load -source http://192.0.2.10:8888/Sun_System_Firmware-9_8_5_c-SPARC_T7-2.pkg
```

```
NOTE: An upgrade takes several minutes to complete. ILOM
      will enter a special mode to load new firmware. No
      other tasks can be performed in ILOM until the
      firmware upgrade is complete and ILOM is reset.

Are you sure you want to load the specified file (y/n)? y
Preserve existing configuration (y/n)? y

Firmware update is complete.
ILOM will now be restarted with the new firmware.
```

```console
-> /sbin/reboot
```


# ILOM - clean registers 

---

Inside the "prompt ok" (OBP = OpenbootProm) use the command bellow to show the registers nu
```console
{0} ok .registers
```

Clean registers:
```console
{0} ok reset-all
```


# Reset SP

---

Link direct to the Oracle documentation.
- <a href="https://docs.oracle.com/cd/E19201-01/820-6412-12/backuprestore_cli.html" target="_blank">`Oracle Integrated Lights Out Manager (ILOM) 3.0 CLI Procedures Guide`</a> 
- <a href="https://docs.oracle.com/cd/E19201-01/820-6412-12/backuprestore_cli.html#50561097_Restore%20the%20ILOM%20Configuration" target="_blank">`Procedure to reset SP`</a> 


# ILOM - POST estendido

---

**Referência**

O documento abaixo fornece os passos de como configurar o post estendido para o nível máximo para o servidor T7-1.  
- <a href="https://docs.oracle.com/cd/E54976_01/html/E54980/z4000ca11317576.html#scrolltoc" target="_blank">`https://docs.oracle.com/cd/E54976_01/html/E54980/z4000ca11317576.html#scrolltoc`</a> 