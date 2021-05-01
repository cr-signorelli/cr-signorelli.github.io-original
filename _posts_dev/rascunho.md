---
title: "Solaris 11.4 - Meu primeiro Solaris Zone"
classes: wide
header:
  overlay_image: /assets/images/solaris11-optimize.jpg
  og_image: /assets/images/solaris11-optimize-og.jpg
  teaser: /assets/images/solaris11-optimize-thumb.jpg
  image_description: "Fundo abstrato colorido vermelho e laranja"
  #caption: "Foto/Imagem: [**signorelli**](https://pixabay.com/)"
#  #actions:
#  #  - label: "Leia mais"
#  #    url: "https://cr-signorelli.github.io/"
categories:
  - solaris-11.4
tags:
  - solaris-11.4
  - x86
  - sparc
  - sysadmin
last_modified_at: 2021-05-01T01:00:00-03:00
---

Solaris Zones é um recurso que oferece um ambiente isolado no qual executar aplicativos no sistema, é um componente do ambiente Solaris Container existente desde Solaris 10.
{: style="text-align: justify;"}

#### Informações

> Utilizando para o laboratório de [Solaris Zones](https://docs.oracle.com/cd/E37838_01/html/E61038/gitsf.html)  
> Virtualizador [Oracle Virtual Box 6.1](https://www.virtualbox.org/)  
> Sistema Operacional [Oracle Solaris 11.4 x86](https://www.oracle.com/solaris/solaris11/downloads/solaris11-install-downloads.html)  

Não irei mostrar o processo de instalação do Solaris 11.4 na máquina virtual para o laboratório de Oracle Solaris Zones.

Para maiores detalhes e sobre a versão, compatibilidade, estrutura consulta a [documentação oficial](https://docs.oracle.com/cd/E37838_01/html/E61037/zonesoverview.html) no site da Oracle.

Confirme a versão do sistema operacional instalado e se suporte a virtualização:

```bash
root@scorpion:~# cat /etc/release
                             Oracle Solaris 11.4 X86
             Copyright (c) 1983, 2021, Oracle and/or its affiliates.
                             Assembled 22 April 2021

root@scorpion:~# virtinfo
NAME            CLASS
virtualbox      current
non-global-zone supported
kernel-zone     unsupported
```

Adicionei um disco para ficar dedicado ao Solaris Zones, não é obrigatório, porém prefiro deixar as cargas separadas, fica a seu gosto !!!

```bash
root@scorpion:~# echo | format
Searching for disks...done

AVAILABLE DISK SELECTIONS:
       0. c1d0 <VBOX HAR-2c963950-811ba19-0001-31.25GB>
          /pci@0,0/pci-ide@1,1/ide@0/cmdk@0,0
          /dev/chassis/SYS/MB/hba0
       1. c1d1 <VBOX HAR-bcc3d0c9-cb1343b-0001-20.00GB>  zones
          /pci@0,0/pci-ide@1,1/ide@0/cmdk@1,0
          /dev/chassis/SYS/MB/hba0
       2. c2d0 <VBOX HAR-6eb41ced-af1a259-0001 cyl 2608 alt 2 hd 255 sec 63>
          /pci@0,0/pci-ide@1,1/ide@1/cmdk@0,0
          /dev/chassis/SYS/MB/hba0
Specify disk (enter its number): Specify disk (enter its number):
root@scorpion:~#
```

Criei um [ZFS](https://docs.oracle.com/cd/E37838_01/html/E61017/index.html) Pool para utilizar o disco dedicado para Solaris Zones: 

```bash
root@scorpion:~# zpool create zones c1d1
root@scorpion:~# zpool status zones
  pool: zones
    id: 18272446593546317300
 state: ONLINE
  scan: none requested
config:

        NAME    STATE      READ WRITE CKSUM
        zones   ONLINE        0     0     0
          c1d1  ONLINE        0     0     0

errors: No known data errors
root@scorpion:~#
root@scorpion:~# df -h /zones
Filesystem             Size   Used  Available Capacity  Mounted on
zones                 19.6G    31K      19.6G     1%    /zones
root@scorpion:~#
```

Vou criar a seguinte estrutura de rede, usarei uma interface dedicada para criar a rede para o Solaris Zone

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/856b5e54-69f0-4944-9e82-29eb089ef417/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/856b5e54-69f0-4944-9e82-29eb089ef417/Untitled.png)


Vamos começar a configurar o nosso primeiro Zone!!!!


```bash
root@scorpion:~# zonecfg -z web
Use 'create' to begin configuring a new zone.
zonecfg:web> create
create: Using system default template 'SYSdefault'
zonecfg:web> set zonepath=/zones/web
zonecfg:web> set autoboot=true
zonecfg:web> verify
zonecfg:web> commit
zonecfg:web> exit
root@scorpion:~#

root@scorpion:~# zonecfg -z web info
zonename: web
zonepath: /zones/web
brand: solaris
autoboot: true
anet:
        linkname: net0
        configure-allowed-address: true
root@scorpion:~#
```


Verificando que a NET1 esta ativa e partimos para criação da VNET1

```bash
root@scorpion:~# ipadm
NAME              CLASS/TYPE STATE        UNDER      ADDR
lo0               loopback   ok           --         --
lo0/v4         static     ok           --         127.0.0.1/8
lo0/v6         static     ok           --         ::1/128
net0              ip         ok           --         --
net0/v4        dhcp       ok           --         192.168.0.79/24
net0/v6        addrconf   ok           --         fe80::a00:27ff:fed1:9a6e/10
net0/v6        addrconf   ok           --         2804:14d:7e23:80f6:a00:27ff:fed1:9a6e/64
net1              ip         ok           --         --
net1/v4        dhcp       ok           --         192.168.0.80/24
root@scorpion:~#

root@scorpion:~# dladm create-vnic -l net1 vnet1

root@scorpion:~# dladm show-vnic
LINK            OVER           SPEED  MACADDRESS        MACADDRTYPE IDS
vnet1           net1           1000   2:8:20:3b:d8:cf   random      VID:0
root@scorpion:~#

root@scorpion:~# dladm show-link
LINK                CLASS     MTU    STATE    OVER
net0                phys      1500   up       --
net1                phys      1500   up       --
vnet1               vnic      1500   up       net1
root@scorpion:~#

```

Com a utilização da VNIC, poderemos customizar interface, por example: o maximo de banda para aquela Zone.

```bash
root@scorpion:~# dladm show-linkprop -p max-bw vnet1
LINK     PROPERTY        PERM VALUE        EFFECTIVE    DEFAULT   POSSIBLE
vnet1    max-bw          rw   --           --           --        --
root@scorpion:~#
```

Vamos remover a interface que vem como padrão e iremos configurar a rede VNET1 no Solaris Zones

```bash
root@scorpion:~# zoneadm list -cv
ID NAME             STATUS      PATH                         BRAND      IP
0 global           running     /                            solaris    shared

root@scorpion:~# zonecfg -z web
zonecfg:web> info
zonename: web
zonepath: /zones/web
brand: solaris
autoboot: true
anet 0:
        linkname: net0
        configure-allowed-address: true
zonecfg:web>
zonecfg:web> remove anet 0
zonecfg:web> info
zonename: web
zonepath: /zones/web
brand: solaris
autoboot: true
zonecfg:web>
zonecfg:web> set ip-type=exclusive
zonecfg:web> add net
zonecfg:web:net> set physical=vnet1
zonecfg:web:net> end
zonecfg:web> verify
zonecfg:web> commit
zonecfg:web> exit
root@scorpion:~#

root@scorpion:~# zonecfg -z web export
create -b
set brand=solaris
set zonepath=/zones/web
set autoboot=true
add net
set physical=vnet1
end
root@scorpion:~#

```

Criar o Filesystem para o Solaris Zones.

```bash
root@scorpion:~# zfs create zones/web
root@scorpion:~# df -h /zones/web
Filesystem             Size   Used  Available Capacity  Mounted on
zones/web             19.6G    31K      19.6G     1%    /zones/web
root@scorpion:~#

```

Antes de realizar a instalação do Solaris Zones, criei um [repositório local](https://www.oracle.com/solaris/solaris11/downloads/local-repository-downloads.html) dos pacotes. Se seguir os passos de criação do repositório local com calma, sai de primeira!!!!

[Solaris 11 Downloads Create a Local Repository](https://www.oracle.com/solaris/solaris11/downloads/local-repository-downloads.html)

Para visualizar o repositório, utilizei o comando #pkg, segue exemplo abaixo:

```bash
root@scorpion:~# pkg publisher
PUBLISHER                   TYPE     STATUS P LOCATION
solaris                     origin   online F file:///repo/solaris/
root@scorpion:~#
```

Vamos Instalar o nosso primeiro Solaris Zonesssssss!!!!! let's Go!!!

```bash
root@scorpion:~# zoneadm list -cv
  ID NAME             STATUS      PATH                         BRAND      IP
   0 global           running     /                            solaris    shared
   - web              configured  /zones/web                   solaris    excl
root@scorpion:~# zoneadm -z web install 
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/64306fb2-8715-4625-bfbb-bddb2152e342/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/64306fb2-8715-4625-bfbb-bddb2152e342/Untitled.png)

Enquanto estiver em instalação, o Zone ficará com o [status](https://www.notion.so/Solaris-Zones-Status-a64c0e38d1594174949aca13d2f13e4c) de incomplete

```bash
root@scorpion:~# zoneadm list -cv
  ID NAME             STATUS      PATH                         BRAND      IP
   0 global           running     /                            solaris    shared
   - web              incomplete  /zones/web                   solaris    excl
root@scorpion:~#
```

 

Após o término da instalação!!!!

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ae53bb0e-5493-44ef-980d-0c50602354ce/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ae53bb0e-5493-44ef-980d-0c50602354ce/Untitled.png)

```bash
root@scorpion:~# zoneadm list -cv
  ID NAME             STATUS      PATH                         BRAND      IP
   0 global           running     /                            solaris    shared
   - web              installed   /zones/web                   solaris    excl
root@scorpion:~#
```

Solaris Zone status

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f567a966-b3ea-4bf4-9433-ce054ac43249/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f567a966-b3ea-4bf4-9433-ce054ac43249/Untitled.png)

Vamos realizar o boot/start do nosso Solaris Zones pela primeira vez! Ops, precisamos pegar a console da nossa máquina virtual para dar sequência na instalação do Solaris.

```bash
root@scorpion:~# zoneadm -z web boot ; zlogin -C web
[Connected to zone 'web' console]
Loading smf(7) service descriptions: 151/151
Booting to milestone "svc:/milestone/config:default".
```

Dependendo do teclado o F2 não funciona, não se preocupe utilize a sequência ESC + 2 

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2ee0e6ac-5d19-454a-a078-51d88d452ad3/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2ee0e6ac-5d19-454a-a078-51d88d452ad3/Untitled.png)

Agora dê um nome para seu Solaris Zone, estava sem criatividade, deixei web mesmo!!!! fica a seu gosto

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/04efc43a-aa41-4943-89a1-1bc1d43d41e0/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/04efc43a-aa41-4943-89a1-1bc1d43d41e0/Untitled.png)

Escolha a interface de rede, nesse caso bem simples!! só tem ela mesmo 

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3237ff27-f804-4382-b588-353afb1c01d9/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3237ff27-f804-4382-b588-353afb1c01d9/Untitled.png)

Eu vou usar a configuração de DHCP, pois estou em meu lab, acredito que vc terá uma lista com IP, Mask, DNS, Gateway.... prepara ela para usar o modo Static

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1e9b89cb-ea1c-412d-9d63-ce18dfbcd74a/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1e9b89cb-ea1c-412d-9d63-ce18dfbcd74a/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bd054574-9539-467b-8656-261df97d4f46/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bd054574-9539-467b-8656-261df97d4f46/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aa568ce7-afe6-4084-8361-22f9c6ff9523/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aa568ce7-afe6-4084-8361-22f9c6ff9523/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/23fabbf9-2a90-4842-a21d-4d70bc8f5455/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/23fabbf9-2a90-4842-a21d-4d70bc8f5455/Untitled.png)

Eu particularmente prefiro deixar em inglês, pois caso precise abrir um chamado, fica mais simples para os técnicos, fica a dica!!!! Se colocar em português, pode ser um problema para buscar na base de conhecimento o erro!!! ;-) 

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2e36c997-f46f-467d-8310-eed7137569d9/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2e36c997-f46f-467d-8310-eed7137569d9/Untitled.png)

Utilizo a language Ingles dos USA, para título de curiosidade a opção -15 é inglês europeu!!!!

[New Oracle Solaris Keyboard Software Support](https://docs.oracle.com/cd/E19253-01/817-2521/new-27660/index.html)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/77aaf2e5-6025-4704-af50-15de007b1fa3/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/77aaf2e5-6025-4704-af50-15de007b1fa3/Untitled.png)

Ficar atento nesse ponto, pois por motivos de segurança, teve algumas mudanças nesse tópico, sugiro ler a [documentação](https://docs.oracle.com/cd/E37838_01/html/E61023/rbactask-21.html), resumindo!!! se apenas adicionar a senha de root, será criado o usuário root, se colocar a senha de root e criar um usuário, esse usuário receberá a role de root e não existirá o usuário root no sistema, espero ter entendido, caso contrário leia a documentação para não ter que reconfigurar o sistema novamente!!!

Porém se precisa reconfigurar o ambiente, veja este artigo

[How to Configure Oracle Solaris 11 Using the sysconfig Command](https://www.oracle.com/technical-resources/articles/it-infrastructure/admin-o11-111-s11-sysconfig.html)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e6790e4c-49f7-4422-be16-7dc9a4337d01/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e6790e4c-49f7-4422-be16-7dc9a4337d01/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f4c68004-a272-4b4d-a842-ebd45494e736/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f4c68004-a272-4b4d-a842-ebd45494e736/Untitled.png)

```bash
SC profile successfully generated as:
/etc/svc/profile/incoming/config/sc_profile.xml

Exiting System Configuration Tool. Log is available at:
/system/volatile/sysconfig.20210430-162341.log.3935
Booting to milestone "all".
Hostname: web

web console login:
```

Para sair do Console do zlogin, utilize a sequencia  digite   ~. <enter>

```bash
 root@web:~# ~.
[Connection to zone 'web' console closed]
root@scorpion:~#
```

Caso não funcione, eu utilizo a sequência de escape do telnet, tecle Ctrl+] <enter>

Outra forma é configurar seu próprio comando de escape, no exemplo usar  .. <enter> 

```bash
root@scorpion:~# zlogin -C -e ".." web
[Connected to zone 'web' console]

web console login: root
Password:
Last login: Fri Apr 30 19:53:51 2021 on pts/2
NOTE: system has 1 active defect; run 'fmadm list' for details.
Apr 30 20:10:16 web login: ROOT LOGIN /dev/console
Oracle Corporation      SunOS 5.11      st_095.server   April 2021
root@web:~#
root@web:~#
root@web:~#
root@web:~# ..
[Connection to zone 'web' console closed]
root@scorpion:~#
```


#### Referências

> [Guidelines for Oracle Solaris Zones in the Oracle Solaris 11.4](https://docs.oracle.com/cd/E37838_01/html/E61038/gitsf.html)  
> [Oracle Virtual Box 6.1](https://www.virtualbox.org/)  
> [Oracle Solaris 11.4 x86](https://www.oracle.com/solaris/solaris11/downloads/solaris11-install-downloads.html)  
> [Introduction to Oracle® Solaris 11.4 Virtual Environments](https://docs.oracle.com/cd/E37838_01/html/E61037/zonesoverview.html)  
> [ZFS](https://docs.oracle.com/cd/E37838_01/html/E61017/index.html)  

