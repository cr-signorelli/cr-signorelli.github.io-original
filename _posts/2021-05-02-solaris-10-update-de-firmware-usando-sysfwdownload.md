---
title: "Solaris 10 - Update de firmware usando sysfwdownload"
classes: wide
permalink: /solaris-10-update-de-firmware-usando-sysfwdownload/
header:
  overlay_filter: 0.5
  overlay_image: https://source.unsplash.com/jXd2FSvcRr8/1920x300
  og_image: https://source.unsplash.com/jXd2FSvcRr8/120x120
  teaser: https://source.unsplash.com/jXd2FSvcRr8/200x100
  image_description: "Placa de computador."
  caption: "Imagem: [**Umberto**](https://unsplash.com/photos/jXd2FSvcRr8)"
  #actions:
  #  - label: "Leia mais"
  #    url: "https://cr-signorelli.github.io/"
excerpt: "Atualizar o firmware em de um servidor por ser muito mais simples, se o Solaris 10 estiver rodando é possível utilizar o comando **sysfwdownload**."
categories:
  - solaris-10
  - ilom
tags:
  - solaris-10
  - sparc
  - sysadmin
  - ilom
show_date: false
date: 2021-05-02T19:00:00-03:00
last_modified_at: 2021-05-02T19:00:00-03:00
---

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
