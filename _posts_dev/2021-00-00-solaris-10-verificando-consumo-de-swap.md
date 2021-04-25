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

# How to check the memory cache consume

---

Server specification for this examples.
- Swap memory = 10GB 
- Physical memory = 28GB 

---

Command to identify hostname, Solaris version, kernel version, and architecture.
```console
-bash-3.2# uname -a
```

Expected outcome.
```console
SunOS SERVERTEST 5.10 Generic_150400-62 sun4v sparc sun4v
```

---

Different command to display swap memory.
```console
-bash-3.2# swap -s
```

Expected outcome.
```console
total: 87832k bytes allocated + 3952k reserved = 91784k used, 33816648k available
```

---

Different command to display swap memory.
```console
-bash-3.2# swap -l
```
Expected outcome.
```console
swapfile             dev  swaplo blocks   free
/dev/dsk/c0d0s1     100,1      16 20496368 20496368
```

---

Different command to display swap memory.
```console
-bash-3.2# vmstat 2 2
```

Expected outcome.
```console
 kthr      memory            page            disk          faults      cpu
 r b w   swap  free  re  mf pi po fr de sr vc -- -- --   in   sy   cs us sy id
 0 0 0 33859920 26366728 46 411 9 1 1 0  0  0  0  0  0 1157  954 1030  0  0 100
 1 0 0 33815328 25780520 30 332 28 0 0 0 0  0  0  0  0 1168 1232 1033  0  0 100
```

---

Command applied in a file system UFS.
```console
-bash-3.2# echo "::memstat" | mdb -k
```

Expected outcome.
```console
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