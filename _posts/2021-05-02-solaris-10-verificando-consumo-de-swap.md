---
title: "Solaris 10 - Verificando consumo de SWAP"
classes: wide
header:
  overlay_image: /assets/images/solaris10.jpg
  og_image: /assets/images/solaris10-og.jpg
  teaser: /assets/images/solaris10-thumb.jpg
  image_description: "Fundo azulado com desenho de meio sol"
  #caption: "Imagem: [signorelli](https://pixabay.com/)"
  #actions:
  #  - label: "Leia mais"
  #    url: "https://cr-signorelli.github.io/"
categories:
  - solaris-10
tags:
  - solaris-10
  - sparc
  - sysadmin
  - swap
  - cache
show_date: false
date: 2021-05-02T19:00:00-03:00
last_modified_at: 2021-05-02T19:00:00-03:00
---

Identificar e monitorar todo consumo de memória é essencial para evitar problemas futuros e garantir o funcionamento e performance correto de um servidor. Embora o Solaris 10 seja um sistema da família Unix, ele possui suas particularidades.
{: style="text-align: justify;"}

Como verificar o consumo de memória cache.
{: style="text-align: justify;"}

Especificações de servidor usado no exemplo:
{: style="text-align: justify;"}

> 10GB = Swap memory
> 28GB = Physical memory

Esse comando mostra as informações de consumo da memória virtual incluindo a memória RAM:
{: style="text-align: justify;"}

```console
-bash-3.2# swap -s

total: 87832k bytes allocated + 3952k reserved = 91784k used, 33816648k available
```

Mostra as informações da area de swap no disco:
{: style="text-align: justify;"}

```console
-bash-3.2# swap -l

swapfile             dev  swaplo blocks   free
/dev/dsk/c0d0s1     100,1      16 20496368 20496368
```

Exibindo estatísticas de memória virtual
{: style="text-align: justify;"}

```console
-bash-3.2# vmstat 2 2

 kthr      memory            page            disk          faults      cpu
 r b w   swap  free  re  mf pi po fr de sr vc -- -- --   in   sy   cs us sy id
 0 0 0 33859920 26366728 46 411 9 1 1 0  0  0  0  0  0 1157  954 1030  0  0 100
 1 0 0 33815328 25780520 30 332 28 0 0 0 0  0  0  0  0 1168 1232 1033  0  0 100
```

Esse comando mostra as estatísticas de consumo de memória de forma mais simples de analisar, infelizmente só funciona em sistema que estão utilizando UFS como filesystem:
{: style="text-align: justify;"}

```console
-bash-3.2# echo "::memstat" | mdb -k

Page Summary                Pages                MB  %Tot
------------     ----------------  ----------------  ----
Kernel                     221753              1732    6%
Anon                        14263               111    0%
Exec and libs                2866                22    0%
Page cache                 212380              1659    6%
Free (cachelist)          3210840             25084   87%
Free (freelist)              7914                61    0%
Total                     3670016             28672
```
