---
title: "Solaris 10 - Boot-read-fail"
classes: wide
header:
  #overlay_color: "#333"
  overlay_image: /assets/images/solaris10-optimize.jpg
  og_image: /assets/images/solaris10-optimize.jpg
  teaser: /assets/images/solaris10-optimize-thumb.jpg
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
last_modified_at: 2021-04-29T18:00:00-03:00
---

Diversos eventos podem desencadear esse problema, afetando os alias dos discos.
{: style="text-align: justify;"}

Embora pareça simples à primeira vista é algo crítico, não conseguir iniciar automaticamente o sistema após uma queda de energia ou uma manutenção programada pode ser um grande inconveniente.
{: style="text-align: justify;"}

**Informações e Requisitos**

- O procedimento descrito foi realizado em um Servidor Oracle T-Series SPARC.
- Servidor dever estar utilizando ILOM como firmware de gerenciamento.
- Acesso de gerenciamento remoto (NET-MGT) ou via console (SER-MGT).
- CD/DVD com a imagem do Solaris 10 ou Imagem ISO.

Acesso o **prompt ok** e realize o boot a maquina com CDROM no modo single user:
{: style="text-align: justify;"}

```console
{ok} boot cdrom -s
```

Após o boot e com sistema carregado é preciso identificar os discos da máquina:
{: style="text-align: justify;"}

```console
# ls /dev/dsk

c0t36479CCB99B8E947d0s0  c0t36479CCB99B8E947d0s6  c0t3866266DF79F5225d0s4  c1t6d0s2  c2t0d0s0  c2t0d0s6
c0t36479CCB99B8E947d0s1  c0t36479CCB99B8E947d0s7  c0t3866266DF79F5225d0s5  c1t6d0s3  c2t0d0s1  c2t0d0s7
c0t36479CCB99B8E947d0s2  c0t3866266DF79F5225d0s0  c0t3866266DF79F5225d0s6  c1t6d0s4  c2t0d0s2
c0t36479CCB99B8E947d0s3  c0t3866266DF79F5225d0s1  c0t3866266DF79F5225d0s7  c1t6d0s5  c2t0d0s3
c0t36479CCB99B8E947d0s4  c0t3866266DF79F5225d0s2  c1t6d0s0                 c1t6d0s6  c2t0d0s4
c0t36479CCB99B8E947d0s5  c0t3866266DF79F5225d0s3  c1t6d0s1                 c1t6d0s7  c2t0d0s5
```

```console
# ls -lh /dev/dsk

lrwxrwxrwx 1 root root  86 Aug 28  2015 c0t36479CCB99B8E947d0s0 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w36479ccb99b8e947,0:a,raw
lrwxrwxrwx 1 root root  86 Aug 28  2015 c0t36479CCB99B8E947d0s1 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w36479ccb99b8e947,0:b,raw
lrwxrwxrwx 1 root root  86 Aug 28  2015 c0t36479CCB99B8E947d0s2 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w36479ccb99b8e947,0:c,raw
lrwxrwxrwx 1 root root  86 Aug 28  2015 c0t36479CCB99B8E947d0s3 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w36479ccb99b8e947,0:d,raw
lrwxrwxrwx 1 root root  86 Aug 28  2015 c0t36479CCB99B8E947d0s4 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w36479ccb99b8e947,0:e,raw
lrwxrwxrwx 1 root root  86 Aug 28  2015 c0t36479CCB99B8E947d0s5 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w36479ccb99b8e947,0:f,raw
lrwxrwxrwx 1 root root  86 Aug 28  2015 c0t36479CCB99B8E947d0s6 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w36479ccb99b8e947,0:g,raw
lrwxrwxrwx 1 root root  86 Aug 28  2015 c0t36479CCB99B8E947d0s7 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w36479ccb99b8e947,0:h,raw
lrwxrwxrwx 1 root root  86 Aug 28  2015 c0t3866266DF79F5225d0s0 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w3866266df79f5225,0:a,raw
lrwxrwxrwx 1 root root  86 Aug 28  2015 c0t3866266DF79F5225d0s1 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w3866266df79f5225,0:b,raw
lrwxrwxrwx 1 root root  86 Aug 28  2015 c0t3866266DF79F5225d0s2 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w3866266df79f5225,0:c,raw
lrwxrwxrwx 1 root root  86 Aug 28  2015 c0t3866266DF79F5225d0s3 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w3866266df79f5225,0:d,raw
lrwxrwxrwx 1 root root  86 Aug 28  2015 c0t3866266DF79F5225d0s4 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w3866266df79f5225,0:e,raw
lrwxrwxrwx 1 root root  86 Aug 28  2015 c0t3866266DF79F5225d0s5 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w3866266df79f5225,0:f,raw
lrwxrwxrwx 1 root root  86 Aug 28  2015 c0t3866266DF79F5225d0s6 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w3866266df79f5225,0:g,raw
lrwxrwxrwx 1 root root  86 Aug 28  2015 c0t3866266DF79F5225d0s7 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w3866266df79f5225,0:h,raw
lrwxrwxrwx 1 root root  72 Aug 28  2015 c1t6d0s0 -> ../../devices/pci@400/pci@2/pci@0/pci@4/scsi@0/iport@40/cdrom@p6,0:a,raw
lrwxrwxrwx 1 root root  72 Aug 28  2015 c1t6d0s1 -> ../../devices/pci@400/pci@2/pci@0/pci@4/scsi@0/iport@40/cdrom@p6,0:b,raw
lrwxrwxrwx 1 root root  72 Aug 28  2015 c1t6d0s2 -> ../../devices/pci@400/pci@2/pci@0/pci@4/scsi@0/iport@40/cdrom@p6,0:c,raw
lrwxrwxrwx 1 root root  72 Aug 28  2015 c1t6d0s3 -> ../../devices/pci@400/pci@2/pci@0/pci@4/scsi@0/iport@40/cdrom@p6,0:d,raw
lrwxrwxrwx 1 root root  72 Aug 28  2015 c1t6d0s4 -> ../../devices/pci@400/pci@2/pci@0/pci@4/scsi@0/iport@40/cdrom@p6,0:e,raw
lrwxrwxrwx 1 root root  72 Aug 28  2015 c1t6d0s5 -> ../../devices/pci@400/pci@2/pci@0/pci@4/scsi@0/iport@40/cdrom@p6,0:f,raw
lrwxrwxrwx 1 root root  72 Aug 28  2015 c1t6d0s6 -> ../../devices/pci@400/pci@2/pci@0/pci@4/scsi@0/iport@40/cdrom@p6,0:g,raw
lrwxrwxrwx 1 root root  72 Aug 28  2015 c1t6d0s7 -> ../../devices/pci@400/pci@2/pci@0/pci@4/scsi@0/iport@40/cdrom@p6,0:h,raw
lrwxrwxrwx 1 root root  90 Aug 28  2015 c2t0d0s0 -> ../../devices/pci@400/pci@2/pci@0/pci@f/pci@0/usb@0,2/hub@2/hub@3/storage@2/disk@0,0:a,raw
lrwxrwxrwx 1 root root  90 Aug 28  2015 c2t0d0s1 -> ../../devices/pci@400/pci@2/pci@0/pci@f/pci@0/usb@0,2/hub@2/hub@3/storage@2/disk@0,0:b,raw
lrwxrwxrwx 1 root root  90 Aug 28  2015 c2t0d0s2 -> ../../devices/pci@400/pci@2/pci@0/pci@f/pci@0/usb@0,2/hub@2/hub@3/storage@2/disk@0,0:c,raw
lrwxrwxrwx 1 root root  90 Aug 28  2015 c2t0d0s3 -> ../../devices/pci@400/pci@2/pci@0/pci@f/pci@0/usb@0,2/hub@2/hub@3/storage@2/disk@0,0:d,raw
lrwxrwxrwx 1 root root  90 Aug 28  2015 c2t0d0s4 -> ../../devices/pci@400/pci@2/pci@0/pci@f/pci@0/usb@0,2/hub@2/hub@3/storage@2/disk@0,0:e,raw
lrwxrwxrwx 1 root root  90 Aug 28  2015 c2t0d0s5 -> ../../devices/pci@400/pci@2/pci@0/pci@f/pci@0/usb@0,2/hub@2/hub@3/storage@2/disk@0,0:f,raw
lrwxrwxrwx 1 root root  90 Aug 28  2015 c2t0d0s6 -> ../../devices/pci@400/pci@2/pci@0/pci@f/pci@0/usb@0,2/hub@2/hub@3/storage@2/disk@0,0:g,raw
lrwxrwxrwx 1 root root  90 Aug 28  2015 c2t0d0s7 -> ../../devices/pci@400/pci@2/pci@0/pci@f/pci@0/usb@0,2/hub@2/hub@3/storage@2/disk@0,0:h,raw
```

Os discos que interessam são os todos que terminam como **s0**:
{: style="text-align: justify;"}

```console
# ls -l /dev/rdsk/*s0

lrwxrwxrwx 1 root root  86 Aug 28  2015 /dev/rdsk/c0t36479CCB99B8E947d0s0 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w36479ccb99b8e947,0:a,raw
lrwxrwxrwx 1 root root  86 Aug 28  2015 /dev/rdsk/c0t3866266DF79F5225d0s0 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w3866266df79f5225,0:a,raw
lrwxrwxrwx 1 root root  72 Aug 28  2015 /dev/rdsk/c1t6d0s0 -> ../../devices/pci@400/pci@2/pci@0/pci@4/scsi@0/iport@40/cdrom@p6,0:a,raw
lrwxrwxrwx 1 root root  90 Aug 28  2015 /dev/rdsk/c2t0d0s0 -> ../../devices/pci@400/pci@2/pci@0/pci@f/pci@0/usb@0,2/hub@2/hub@3/storage@2/disk@0,0:a,raw
```

Descarte o devices que não interessam como CDROM e USB:
{: style="text-align: justify;"}

```console
# ls -l /dev/rdsk/*s0 | grep -v cdrom | grep -v usb

lrwxrwxrwx 1 root root  86 Aug 28  2015 /dev/rdsk/c0t36479CCB99B8E947d0s0 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w36479ccb99b8e947,0:a,raw
lrwxrwxrwx 1 root root  86 Aug 28  2015 /dev/rdsk/c0t3866266DF79F5225d0s0 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w3866266df79f5225,0:a,raw
```

Agora vamos refazer o boot:
{: style="text-align: justify;"}

```console
# installboot -F zfs /usr/platform/`uname -i`/lib/fs/zfs/bootblk /dev/rdsk/c0t3866266DF79F5225d0s0
# installboot -F zfs /usr/platform/`uname -i`/lib/fs/zfs/bootblk /dev/rdsk/c0t36479CCB99B8E947d0s0
```

Delisge o sistema e volte para o **prompt ok** e verifica qual é a controladora SCSI:
{: style="text-align: justify;"}

```console
{0} ok probe-scsi-all
```

Seleciona a controladora SCSI
{: style="text-align: justify;"}

```console
{0} ok select /pci@400/pci@1/pci@0/pci@4/scsi@0
```

Redefina os alias fixar as configurações, atenção ao montar a linha de comando:  
**_/pci@400/pci@1/pci@0/pci@4/scsi@0 --> path da controladora_**  
**_disk@w3866266df79f5225,0:a  --> disco identificado anteriormente_**  

```console
{0} ok nvalias rootdisk /pci@400/pci@1/pci@0/pci@4/scsi@0/disk@w3866266df79f5225,0:a
{0} ok nvalias rootmirr /pci@400/pci@1/pci@0/pci@4/scsi@0/disk@w36479ccb99b8e947,0:a
{0} ok setenv boot-device rootdisk rootmirr
```

Também é possível realizar o procedimento com  o sistema operacional (Solaris 10) ativo:
{: style="text-align: justify;"}

```console
-bash-3.2# eeprom boot-device="rootdisk rootmirr"
-bash-3.2# eeprom multipath-boot?=false
-bash-3.2# eeprom boot-device-index=0
-bash-3.2# eeprom use-nvramrc?=true
-bash-3.2# eeprom nvramrc="devalias rootdisk /pci@400/pci@1/pci@0/pci@4/scsi@0/disk@w3866266df79f5225,0:a devalias rootmirr /pci@400/pci@1/pci@0/pci@4/scsi@0/disk@w36479ccb99b8e947,0:a"
```

**Aviso:** Se sua empresa possui contrato de suporte ativo ou o equipamento está em garantia faça abertura de chamado no _[My Oracle Support](https://support.oracle.com/portal/)_, além de simples é possível realizar todo atendimento online. Poder contar com uma equipe de suporte qualificada como apoio é indispensável.
{: .notice--warning}

#### Referências

> [Lista de Servidores Oracle T-Series](https://en.wikipedia.org/wiki/SPARC_T_series)  
> [Integrated Lights Out Manager (ILOM)](https://docs.oracle.com/cd/E19860-01/E21549/z400000c1393879.html)  
> [Advanced Lights Out Manager (ALOM)](https://docs.oracle.com/cd/E19088-01/v125.srvr/819-2445-11/819-2445-11.pdf)  
