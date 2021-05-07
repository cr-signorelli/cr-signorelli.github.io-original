---
title: "Solaris 10 - Update de firmware usando sysfwdownload"
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
  - ilom
tags:
  - solaris-10
  - sparc
  - sysadmin
  - ilom
date: 2021-05-02T19:00:00-03:00
last_modified_at: 2021-05-02T19:00:00-03:00
---

Atualizar o firmware em de um servidor por ser muito mais simples, se o Solaris 10 estiver rodando é possível utilizar o comando **sysfwdownload**.
{: style="text-align: justify;"}

**Alerta:** Mesmo realizando o processo de upgrade via software será preciso reiniciar o servidor.
{: .notice--danger}

Primeiro verifique qual a versão de SunSystem Firmware:
{: style="text-align: justify;"}

```console
-bash-3.2# ./sysfwdownload -g
```

Inice o processo de upgrade:
{: style="text-align: justify;"}

```console
-bash-3.2# ./sysfwdownload -u [image].pkg
```

Antes de continuar o sistema precisa da sua confirmação de que será reiniciado após o processo:
{: style="text-align: justify;"}

```console
WARNING: Host will be powered down for automatic firmware update when download is completed.
Do you want to continue(yes/no)?
```

Depois de finalizar o processo de upgrade e reiniciar o sistema, acessa novamente e confirme se o firmware está correto:
{: style="text-align: justify;"}

```console
-bash-3.2# ./sysfwdownload -g
```

**Dica:** Caso o firmware do servidor esteja muito antigo, será preciso realizar o processo de upgrade até uma versão compatível com a atual.
{: .notice--info}

#### Referências

> [Download the firmware](https://www.oracle.com/servers/technologies/firmware/release-history-jsp.html){:target="_blank"}  
