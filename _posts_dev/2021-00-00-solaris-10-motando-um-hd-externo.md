---
title: "Solaris 10 - Montando HD externo"
classes: wide
header:
  overlay_image: /assets/images/solaris10-optimize.jpg
  og_image: /assets/images/solaris10-optimize-og.jpg
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
  - network
  - instalacao
last_modified_at: 2021-05-01T23:00:00-03:00
---

Caso haja necessidade de adicionar um HD externo esse procedimento irá auxilia-lo a como localizar, montar e preparar o filesystem do disco novo.

Forçe o sistema operacional a procurar novos devices USB:

```console
iostat -En
```

Verifique qual é o drive externo:

```console
rmformat -l
```

Crie uma partição primária:

```console
fdisk /dev/rdsk/c3t0d0p0
```

Verifique a quantidade de setores:

```console
prtvtoc /dev/rdsk/c3t0d0p0
```

Crie o file system (esse procedimento apaga todos os dados):

```console
newfs -v -b 8192 -i 16384 -s 3906745920 /dev/rdsk/c3t0d0p0
```

Montando o file system em um diretório:

```console
mount /dev/dsk/c3t0d0s2 /opt/iomega
```

Montando o disco como volume lógico (ZFS):

```console
zpool create -R /opt/iomega -f iomega /dev/dsk/c3t0d0
```

Ativando compressão do disco (isso afeta a performance):

```console
zfs set compression=on iomega
```
