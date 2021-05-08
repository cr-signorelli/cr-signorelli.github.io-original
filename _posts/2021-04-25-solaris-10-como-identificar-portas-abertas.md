---
title: "Solaris 10 - Como identificar portas abertas"
classes: wide
permalink: /solaris-10-como-identificar-portas-abertas/
header:
  overlay_filter: 0.5
  overlay_image: https://source.unsplash.com/bBavss4ZQcA/1920x300
  og_image: https://source.unsplash.com/bBavss4ZQcA/120x120
  teaser: https://source.unsplash.com/bBavss4ZQcA/200x100
  image_description: "Grade vermelha com cadeados presos."
  caption: "Imagem: [**Jon Moore**](https://unsplash.com/photos/bBavss4ZQcA)"
  #actions:
  #  - label: "Leia mais"
  #    url: "https://cr-signorelli.github.io/"
excerpt: "Além de identificar as portas abertas no sistema sabia como associá-las ao serviço correspondente."
categories:
  - solaris-10
tags:
  - solaris-10
  - sparc
  - sysadmin
  - segurança
  - network
show_date: false
date: 2021-04-25T15:00:00-03:00
last_modified_at: 2021-04-25T15:00:00-03:00
---

Identificar e conhecer todas as portas abertas do sistema operacional é algo essencial. Serviços desconhecidos, portas abertas sem controle podem significar ou torne-se uma fonte de problemas como funcionamento, performance, vulnerabilidade dentre outros. 
{: style="text-align: justify;"}

Para listar as portas do sistema no Solaris 10 podemos usar o comando **netstat**, mas no exemplo abaixo vamos realizar o processo um pouco diferente, além de identificar as portas vamos associá-las ao processo que as estão mantendo e seu PID.
{: style="text-align: justify;"}

Use a combinação de comandos para listar as portas abertas:
{: style="text-align: justify;"}

```console
-bash-3.2#  cd /proc ; /usr/proc/bin/pfiles * | egrep "^[0-9]|sockname" | more 
```

O comando acima mostrara uma lista de portas em uso. A partir disso voce tera algo como:
{: style="text-align: justify;"}

```console
2287: /usr/sfw/sbin/snmpd
sockname: AF_INET 0.0.0.0 port: 161
sockname: AF_INET 0.0.0.0 port: 33543
sockname: AF_INET 0.0.0.0 port: 0
```

Veja que o número **2287** que esta a esquerda, esse é o PID do processo que está usando a porta 161.
{: style="text-align: justify;"}

Com esses dados é possível localizar o processo usando o comando **ps** e levantar mais detalhes.
{: style="text-align: justify;"}

```console
-bash-3.2# ps -ef | grep 2287
root 2287 1 0 Oct 10 ? 0:16 /usr/sfw/sbin/snmpd
```

**Alerta:** Simplesmente localizar a parar um processo sem entender qual sua função pode gerar problemas, além de que, existem ferramentas que monitoram podem religá-lo.
{: .notice--danger}