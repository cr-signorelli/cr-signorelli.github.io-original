---
title: "Solaris 10 - Espelhamento de disco no ZFS"
classes: wide
permalink: /2021-05-10-solaris-10-espelhamento-de-disco-no-zfs/
header:
  # overlay_filter: 0.5
  overlay_image: https://source.unsplash.com/hsg538WrP0Y/1920x300
  og_image: https://source.unsplash.com/hsg538WrP0Y/120x120
  teaser: https://source.unsplash.com/hsg538WrP0Y/200x100
  image_description: "Foto de um prédio espelhado"
  caption: "Imagem: [**drmakete lab**](https://unsplash.com/photos/hsg538WrP0Y)"
  #actions:
  #  - label: "Leia mais"
  #    url: "https://cr-signorelli.github.io/"
excerpt: "Como fazer espelhamento de disco que estão utilizando o ZFS - Zettabyte file system."
categories:
  - solaris-10
tags:
  - solaris-10
  - sparc
  - sysadmin
  - disk
show_date: false
date: 2021-05-02T19:00:00-03:00
last_modified_at: 2021-05-10T21:00:00-03:00
---

Existem diferentes formas de realizar o espelhamento de disco, via hardware com uma controladora SCSI ou software. Em ambos os casos existem vantagens e desvantagem antes de tomar qualquer decisão é preciso analisar qual seu cenário, pense no futuro quando for necessário mexer nessa estrutura.
{: style="text-align: justify;"}

Especificações que vamos usar para o exemplo:  
{: style="text-align: justify;"}

> c0t361CEB4725D71723d0s0 = disco principal, onde está instalado o sistema operacional
> c0t3B7CA031337740CDd0s0 = disco secundário

**Dica:** A identificação dos disco do exemplo podem parecer um pouco diferentes, isso porque são volumes logicos em vez de discos físicos.
{: .notice--info}

Primeiro localize os discos reconhecidos pelo sitema:
{: style="text-align: justify;"}

```console
bash-3.2# ls /dev/rdsk/*s0

/dev/rdsk/c0t361CEB4725D71723d0s0   /dev/rdsk/c0t3B7CA031337740CDd0s0   /dev/rdsk/c1t6d0s0   /dev/rdsk/c2t0d0s0
```

Comando para realizar o espelhanto dos discos:
{: style="text-align: justify;"}


```console
bash-3.2# zpool attach -f rpool c0t361CEB4725D71723d0s0 c0t3B7CA031337740CDd0s0
```

Em caso de erro no espelhamento do RAID, será necessário acertar os slices dos disco novo:
{: style="text-align: justify;"}


```console
bash-3.2# zpool attach -f rpool c0t361CEB4725D71723d0s0 c0t3B7CA031337740CDd0s0

    cannot attach c0t3B7CA031337740CDd0s0 to c0t361CEB4725D71723d0s0: device is too small
```

Verificar os slices do disco 1:
{: style="text-align: justify;"}

```console
bash-3.2# prtvtoc /dev/rdsk/c0t361CEB4725D71723d0s0
    * /dev/rdsk/c0t361CEB4725D71723d0s0 partition map
    *
    * Dimensions:
    *     512 bytes/sector
    *     139 sectors/track
    *     128 tracks/cylinder
    *   17792 sectors/cylinder
    *   65535 cylinders
    *   65533 accessible cylinders
    *
    * Flags:
    *   1: unmountable
    *  10: read-only
    *
    *                          First     Sector    Last
    * Partition  Tag  Flags    Sector     Count    Sector  Mount Directory
           0      2    00          0 1165963136 1165963135
           2      5    00          0 1165963136 1165963135
```

Verificar os slices do disco 2:
{: style="text-align: justify;"}

```console
bash-3.2# prtvtoc /dev/rdsk/c0t3B7CA031337740CDd0s0
    * /dev/rdsk/c0t3B7CA031337740CDd0s0 partition map
    *
    * Dimensions:
    *     512 bytes/sector
    *     139 sectors/track
    *     128 tracks/cylinder
    *   17792 sectors/cylinder
    *   65535 cylinders
    *   65533 accessible cylinders
    *
    * Flags:
    *   1: unmountable
    *  10: read-only
    *
    *                          First     Sector    Last
    * Partition  Tag  Flags    Sector     Count    Sector  Mount Directory
           0      2    00          0    266880    266879
           1      3    01     266880    266880    533759
           2      5    01          0 1165963136 1165963135
           6      4    00     533760 1165429376 1165963135
```

Veja que os slices estão diferentes, nesse caso é preciso ajustar isso antes de continuar o processo. O jeito mais fácil é criar um modelo baseado no HD principal e aplicá-lo no disco novo.
Vamos criar um arquivo de modelo.
{: style="text-align: justify;"}

Criando um modelo basedo nos slices do disco principal:
{: style="text-align: justify;"}

```console
bash-3.2# prtvtoc /dev/rdsk/c0t361CEB4725D71723d0s0 > /tmp/volume1.out
```

Aplicando o modelo no disco secundário:
{: style="text-align: justify;"}

```console
bash-3.2# fmthard -s /tmp/volume1.out /dev/rdsk/c0t3B7CA031337740CDd0s0

    fmthard:  New volume table of contents now in place.
```

Verificar os slices do disco 1 novamente:
{: style="text-align: justify;"}

```console
bash-3.2# prtvtoc /dev/rdsk/c0t361CEB4725D71723d0s0
    * /dev/rdsk/c0t361CEB4725D71723d0s0 partition map
    *
    * Dimensions:
    *     512 bytes/sector
    *     139 sectors/track
    *     128 tracks/cylinder
    *   17792 sectors/cylinder
    *   65535 cylinders
    *   65533 accessible cylinders
    *
    * Flags:
    *   1: unmountable
    *  10: read-only
    *
    *                          First     Sector    Last
    * Partition  Tag  Flags    Sector     Count    Sector  Mount Directory
           0      2    00          0 1165963136 1165963135
           2      5    00          0 1165963136 1165963135
```

Verificar os slices do disco 2 novamente:
{: style="text-align: justify;"}

```console
bash-3.2# prtvtoc /dev/rdsk/c0t3B7CA031337740CDd0s0
    * /dev/rdsk/c0t3B7CA031337740CDd0s0 partition map
    *
    * Dimensions:
    *     512 bytes/sector
    *     139 sectors/track
    *     128 tracks/cylinder
    *   17792 sectors/cylinder
    *   65535 cylinders
    *   65533 accessible cylinders
    *
    * Flags:
    *   1: unmountable
    *  10: read-only
    *
    *                          First     Sector    Last
    * Partition  Tag  Flags    Sector     Count    Sector  Mount Directory
           0      2    00          0 1165963136 1165963135
           2      5    00          0 1165963136 1165963135
```

Agora é possivel continuar com attach nos discos:

```console
bash-3.2# zpool attach -f rpool c0t361CEB4725D71723d0s0 c0t3B7CA031337740CDd0s0

    Make sure to wait until resilver is done before rebooting.
```

Utilizando o comando **zpool status** acompanhe o processo de espelhamento.

**Aviso:** Quanto mais ocupado estiver o disco principal, mais lento e demorado será o processo de sincronismo. Além disso durante o processo de sincronismo a performance do servidor será afetada consideravelmente.
{: .notice--warning}

```console
bash-3.2# zpool status
      pool: rpool
     state: ONLINE
    status: One or more devices is currently being resilvered.  The pool will
            continue to function, possibly in a degraded state.
    action: Wait for the resilver to complete.
     scan: resilver in progress since Mon Apr 16 12:49:37 2012
        3.46G scanned out of 6.54G at 111M/s, 0h0m to go
        3.46G scanned out of 6.54G at 111M/s, 0h0m to go
        3.46G resilvered, 52.95% done
    config:
    
            NAME                         STATE     READ WRITE CKSUM
            rpool                        ONLINE       0     0     0
              mirror-0                   ONLINE       0     0     0
                c0t361CEB4725D71723d0s0  ONLINE       0     0     0
                c0t3B7CA031337740CDd0s0  ONLINE       0     0     0  (resilvering)
    
    errors: No known data errors
```

Antes de fazer qualquer outra coisa aguarde o "resilvering" dos disco:
{: style="text-align: justify;"}

```console
bash-3.2# zpool status
      pool: rpool
     state: ONLINE
    status: One or more devices is currently being resilvered.  The pool will
            continue to function, possibly in a degraded state.
    action: Wait for the resilver to complete.
     scan: resilver in progress since Mon Apr 16 12:49:37 2012
        6.50G scanned out of 6.54G at 63.4M/s, 0h0m to go
        6.50G scanned out of 6.54G at 63.4M/s, 0h0m to go
        6.50G resilvered, 99.36% done
    config:
    
            NAME                         STATE     READ WRITE CKSUM
            rpool                        ONLINE       0     0     0
              mirror-0                   ONLINE       0     0     0
                c0t361CEB4725D71723d0s0  ONLINE       0     0     0
                c0t3B7CA031337740CDd0s0  ONLINE       0     0     0  (resilvering)
    
    errors: No known data errors
```

Finalizado o resilvering e com isso o espelhamento do disco está completo:
{: style="text-align: justify;"}

```console
bash-3.2# zpool status
      pool: rpool
     state: ONLINE
     scan: resilvered 6.54G in 0h1m with 0 errors on Mon Apr 16 12:51:23 2012
     config:
    
            NAME                         STATE     READ WRITE CKSUM
            rpool                        ONLINE       0     0     0
              mirror-0                   ONLINE       0     0     0
                c0t361CEB4725D71723d0s0  ONLINE       0     0     0
                c0t3B7CA031337740CDd0s0  ONLINE       0     0     0
    
    errors: No known data errors
```
