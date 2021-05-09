---
title: "Solaris 10 - Montando HD externo"
classes: wide
permalink: /solaris-10-motando-um-hd-externo/
header:
  overlay_image: https://source.unsplash.com/sIX4eDtak7k/1920x300
  og_image: https://source.unsplash.com/sIX4eDtak7k/120x120
  teaser: https://source.unsplash.com/sIX4eDtak7k/200x100
  image_description: "Parte interna de uma HD de computador."
  caption: "Imagem: [**Sajad Nori**](https://unsplash.com/photos/sIX4eDtak7k)"
  #actions:
  #  - label: "Leia mais"
  #    url: "https://cr-signorelli.github.io/"
excerpt: "Caso haja necessidade de adicionar um HD externo esse procedimento irá auxilia-lo a como localizar, montar e preparar o filesystem do disco novo."
categories:
  - solaris-10
tags:
  - solaris-10
  - sparc
  - sysadmin
  - network
  - instalacao
show_date: false
date: 2021-05-02T00:30:00-03:00
last_modified_at: 2021-05-02T00:30:00-03:00
---

Forçe o sistema operacional a procurar novos devices USB:
{: style="text-align: justify;"}

```console
iostat -En
```

Verifique qual é o drive externo:
{: style="text-align: justify;"}

```console
rmformat -l
```

Crie uma partição primária:
{: style="text-align: justify;"}

```console
fdisk /dev/rdsk/c3t0d0p0
```

Verifique a quantidade de setores:
{: style="text-align: justify;"}

```console
prtvtoc /dev/rdsk/c3t0d0p0
```

Crie o file system (esse procedimento apaga todos os dados):
{: style="text-align: justify;"}

```console
newfs -v -b 8192 -i 16384 -s 3906745920 /dev/rdsk/c3t0d0p0
```

Montando o file system em um diretório:
{: style="text-align: justify;"}

```console
mount /dev/dsk/c3t0d0s2 /opt/iomega
```

Montando o disco como volume lógico (ZFS):
{: style="text-align: justify;"}

```console
zpool create -R /opt/iomega -f iomega /dev/dsk/c3t0d0
```

Ativando compressão do disco (isso afeta a performance):
{: style="text-align: justify;"}

```console
zfs set compression=on iomega
```
