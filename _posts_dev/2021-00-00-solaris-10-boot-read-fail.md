---
title: "Solaris 10 - Boot-read-fail"
classes: wide
header:
  overlay_color: "#333"
  #overlay_image: /assets/images/solaris-optmize.jpg
  #og_image: /assets/images/solaris-optmize.jpg
  #teaser: /assets/images/solaris-optmize-thumb.jpg
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

# 

---

**Procedimento para tratar boot-read fail**

---

Se possivel boot a maquina com CDROM como single user.
```console
{ok} boot cdrom -s
```

```console
# ls /dev/dsk
c0t36479CCB99B8E947d0s0  c0t36479CCB99B8E947d0s6  c0t3866266DF79F5225d0s4  c1t6d0s2                 c2t0d0s0                 c2t0d0s6
c0t36479CCB99B8E947d0s1  c0t36479CCB99B8E947d0s7  c0t3866266DF79F5225d0s5  c1t6d0s3                 c2t0d0s1                 c2t0d0s7
c0t36479CCB99B8E947d0s2  c0t3866266DF79F5225d0s0  c0t3866266DF79F5225d0s6  c1t6d0s4                 c2t0d0s2
c0t36479CCB99B8E947d0s3  c0t3866266DF79F5225d0s1  c0t3866266DF79F5225d0s7  c1t6d0s5                 c2t0d0s3
c0t36479CCB99B8E947d0s4  c0t3866266DF79F5225d0s2  c1t6d0s0                 c1t6d0s6                 c2t0d0s4
c0t36479CCB99B8E947d0s5  c0t3866266DF79F5225d0s3  c1t6d0s1                 c1t6d0s7                 c2t0d0s5
```

```console
# ls -lh
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

Os discos que interessam são os todos que terminam como s0.
```console
# ls -l /dev/rdsk/*s0
lrwxrwxrwx 1 root root  86 Aug 28  2015 /dev/rdsk/c0t36479CCB99B8E947d0s0 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w36479ccb99b8e947,0:a,raw
lrwxrwxrwx 1 root root  86 Aug 28  2015 /dev/rdsk/c0t3866266DF79F5225d0s0 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w3866266df79f5225,0:a,raw
lrwxrwxrwx 1 root root  72 Aug 28  2015 /dev/rdsk/c1t6d0s0 -> ../../devices/pci@400/pci@2/pci@0/pci@4/scsi@0/iport@40/cdrom@p6,0:a,raw
lrwxrwxrwx 1 root root  90 Aug 28  2015 /dev/rdsk/c2t0d0s0 -> ../../devices/pci@400/pci@2/pci@0/pci@f/pci@0/usb@0,2/hub@2/hub@3/storage@2/disk@0,0:a,raw
```

Aqui estão nossos discos.
```
/dev/rdsk/c0t36479CCB99B8E947d0s0 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w36479ccb99b8e947,0:a,raw
/dev/rdsk/c0t3866266DF79F5225d0s0 -> ../../devices/pci@400/pci@1/pci@0/pci@4/scsi@0/iport@v0/disk@w3866266df79f5225,0:a,raw
```

Agora vamos refazer o boot.
```console
# installboot -F zfs /usr/platform/`uname -i`/lib/fs/zfs/bootblk /dev/rdsk/c0t3866266DF79F5225d0s0
# installboot -F zfs /usr/platform/`uname -i`/lib/fs/zfs/bootblk /dev/rdsk/c0t36479CCB99B8E947d0s0
```

Volte para o prompt ok e verifica qual é a controladora SCSI.
```console
{0} ok probe-scsi-all
```

Seleciona a controladora SCSI
```console
{0} ok select /pci@400/pci@1/pci@0/pci@4/scsi@0
```

Redefina os alias fixar as configurações.
```console
{0} ok nvalias rootdisk /pci@400/pci@1/pci@0/pci@4/scsi@0/disk@w3866266df79f5225,0:a
{0} ok nvalias rootmirr /pci@400/pci@1/pci@0/pci@4/scsi@0/disk@w36479ccb99b8e947,0:a
{0} ok setenv boot-device rootdisk rootmirr
```

Caso os sistema operacional esteja ligado faça pelo Sistema Operacional.
```console
# eeprom boot-device="rootdisk rootmirr"
# eeprom multipath-boot?=false
# eeprom boot-device-index=0
# eeprom use-nvramrc?=true
# eeprom nvramrc="devalias rootdisk /pci@400/pci@1/pci@0/pci@4/scsi@0/disk@w3866266df79f5225,0:a devalias rootmirr /pci@400/pci@1/pci@0/pci@4/scsi@0/disk@w36479ccb99b8e947,0:a"
```