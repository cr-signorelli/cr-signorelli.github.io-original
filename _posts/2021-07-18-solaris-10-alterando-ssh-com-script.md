---
title: "Solaris 10 - Alterando SSH com script"
classes: wide
permalink: /solaris-10-alterando-ssh-com-script/
header:
  overlay_filter: 0.5
  overlay_image: https://source.unsplash.com/FWoq_ldWlNQ/1920x300
  og_image: https://source.unsplash.com/FWoq_ldWlNQ/120x120
  teaser: https://source.unsplash.com/FWoq_ldWlNQ/200x100
  image_description: "Foto de um prédio espelhado"
  caption: "Imagem: [**Mitchell Luo**](https://unsplash.com/photos/FWoq_ldWlNQ)"
  #actions:
  #  - label: "Leia mais"
  #    url: "https://cr-signorelli.github.io/"
excerpt: "Como alterar o sshd_config utilizando comando ed com script."
categories:
  - solaris-10
tags:
  - solaris-10
  - sparc
  - sysadmin
  - segurança
  - shellscript
show_date: false
date: 2021-07-18T21:00:00-03:00
last_modified_at: 2021-07-18T21:00:00-03:00
---



O Solaris 10 por padrão não tem a versão mais nova do comando **sed** que possui a opção **-i** (interactive) com ela é possível alterar o conteúdo do arquivo. Além disso é muito comum entrar ou ter de realizar instalações com algum tipo de [Hardening](https://pt.wikipedia.org/wiki/Hardening){:target="_blank"}, por exemplo instalar apenas o básico do sistema operacional e completar sua instalação depois apenas com o mínimo necessário. Dessa forma é possível zelar pela performance e segurança do sistema ao mesmo tempo.
{: style="text-align: justify;"}

Bom agora que com esses detalhes imagino que é fácil entender por que usar um comando simples tão básico como **ed** pode ser muito útil para alterar o conteúdo de um arquivo.
{: style="text-align: justify;"}

Como os scripts abaixo é possível habilitar as opções **X11 forwarding** e o **PermitRootLogin** no arquivo **/etc/ssh/sshd_config**.
{: style="text-align: justify;"}

**Alerta:** Habilite essas opções apenas de casos de extrema necessidade e de forma temporária, ou seja, desative o mais rápido possível.  
{: .notice--danger}

Habilita as opções[ed-ssh-config-yes.sh](https://github.com/cr-signorelli/solaris10/blob/master/scripts/ed-ssh-config-yes.sh){:target="_blank"}:
{: style="text-align: justify;"}

```console
#!/bin/sh

ed - << EOF
r /etc/ssh/sshd_config
/PermitRootLogin/
c
PermitRootLogin yes
.
/X11Forwarding/
c
X11Forwarding yes
.
w
q
EOF
```

Desabilita as opções[ed-ssh-config-no.sh](https://github.com/cr-signorelli/solaris10/blob/master/scripts/ed-ssh-config-no.sh){:target="_blank"}:
{: style="text-align: justify;"}

```console
#!/bin/sh

ed - << EOF
r /etc/ssh/sshd_config
/PermitRootLogin/
c
PermitRootLogin no
.
/X11Forwarding/
c
X11Forwarding no
.
w
q
EOF
```

#### Referências

> [Hardening](https://pt.wikipedia.org/wiki/Hardening){:target="_blank"}  
> [Script ed-ssh-config-yes.sh](https://github.com/cr-signorelli/solaris10/blob/master/scripts/ed-ssh-config-yes.sh){:target="_blank"}  
> [Script ed-ssh-config-no.sh](https://github.com/cr-signorelli/solaris10/blob/master/scripts/ed-ssh-config-no.sh){:target="_blank"}  
