---
title: "DNS Bind9 - extraindo conteúdo de uma arquivo RAW"
classes: wide
header:
  overlay_image: /assets/images/solaris10.jpg
  og_image: /assets/images/solaris10-og.jpg
  teaser: /assets/images/solaris10-thumb.jpg
  image_description: "Fundo azulado com desenho de meio sol"
  #caption: "Imagem: [**signorelli**](https://pixabay.com/)"
  #actions:
  #  - label: "Leia mais"
  #    url: "https://cr-signorelli.github.io/"
categories:
  - solaris-10
  - dns
tags:
  - solaris-10
  - dns
  - bind9
  - sparc
  - sysadmin
date: 2021-04-22T23:00:00-03:00
last_modified_at: 2021-04-22T23:00:00-03:00
---

DNS Bind9 - extraindo conteúdo de uma arquivo RAW

Como abrir o conteúdo de um arquivo de zone em formato RAW

```console
/usr/local/sbin/named-compilezone -f raw -F text -o arquivo.txt dominio.com.br. dominio.com.br.zone
```

Detalhamento do comando:  

|                                   |                                           |
|-----------------------------------|-------------------------------------------|
| /usr/local/sbin/named-compilezone | comando                                   |
| -f raw -F text                    | formato de raw para txt                   |
| -o                                | saída                                     |
| arquivo.txt                       | arquivo texto                             |
| dominio.com.br.                   | dominio                                   |
| dominio.com.br.zone2              | arquivo de zona com o conteudo do dominio |   

Como converter um arquivo de TXT para RAW:

```console
/usr/local/sbin/named-compilezone -f text -F raw -o dominio.com.br.zone dominio.com.br arquivo.txt
```
