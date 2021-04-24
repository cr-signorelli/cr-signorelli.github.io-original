---
title: "Solaris 10 - Removendo e instalando cliente SSH"
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
  - segurança
last_modified_at: 2021-04-23T23:00:00-03:00
---

Remover os clientes SSH podem ser uma medida de segurança agressiva, é importante ressaltar que isso pode trazer vantagens e desvantagens.
{: style="text-align: justify;"}

Embora seja uma medida extrema, se considerar que uma vez conectado ao servidor não seja necessário acessar outras máquinas realmente programas como clientes SSH ou SCP não são necessários. Mas ao fazer isso você também vai afetar suas atividades como transferir ou coletar algum arquivo ou log de um servidor.
{: style="text-align: justify;"}

Caso haja invasão isso será mais uma barreira, mas em muitos casos os binários de sistema são as primeiras vítimas, normalmente trocados por versão alteradas dependendo da estratégia e intensão.
{: style="text-align: justify;"}

Veja a lista completa dos binários e arquivos que fazem parte dos pacotes **SUNWsshr** e **SUNWsshu**.
{: style="text-align: justify;"}

```console
etc/ssh
etc/ssh/moduli
etc/ssh/ssh_config
usr/bin/scp
usr/bin/sftp
usr/bin/ssh
usr/bin/ssh-add
usr/bin/ssh-agent
usr/lib/ssh
usr/lib/ssh/ssh-http-proxy-connect
usr/lib/ssh/ssh-socks5-proxy-connect
```

#### Removendo o cliente SSH

Verifique se os pacotes estão instalados na máquina:
{: style="text-align: justify;"}

```console
-bash-3.2# pkginfo | grep ssh

system      SUNWsshcu                    SSH Common, (Usr)
system      SUNWsshdr                    SSH Server, (Root)
system      SUNWsshdu                    SSH Server, (Usr)
system      SUNWsshr                     SSH Client and utilities, (Root)
system      SUNWsshu                     SSH Client and utilities, (Usr)
```

Uma vez localizdos use o comando **pkgrm** para removê-los:
{: style="text-align: justify;"}

```console
-bash-3.2# pkgrm -n SUNWsshr SUNWsshu
```

Veja a lista novamente para certificar-se de não estão mais presentes:
{: style="text-align: justify;"}

```console
-bash-3.2# pkginfo | grep ssh

system      SUNWsshcu                    SSH Common, (Usr)
system      SUNWsshdr                    SSH Server, (Root)
system      SUNWsshdu                    SSH Server, (Usr)
```

#### Reinstalando o cliente SSH

É possível realizar esse processo de algumas formas diferentes:
{: style="text-align: justify;"}

1. *CD/DVD*
2. *Imagem .ISO*
3. *Arquivos .pkg*

Não iremos utilizar a primeira opção, é muito comum que servidores não tenham unidade de CD/DVD e vamos considerar que acessar a máquina fisicamente para inserir um disco nem sempre é uma opção. Por isso vamos focar no procedimento da imagem .ISO.
{: style="text-align: justify;"}

Baixe a imagem ISO do Solaris-10 Sparc/x86 no site oficial da Oracle.  
<https://www.oracle.com/solaris/solaris10/downloads/solaris10-get-jsp-downloads.html>

Associe o arquivo **sol-10-u11-ga-sparc-dvd.iso** em um "loopback file driver":  
{: style="text-align: justify;"}

```console
-bash-3.2#  lofiadm -a /tmp/sol-10-u11-ga-sparc-dvd.iso 
```

Monte o dispositivo virtual e acesse o diretório:
{: style="text-align: justify;"}

```console
-bash-3.2# mount -F hsfs -o ro /dev/lofi/1 /mnt
-bash-3.2# cd /mnt/Solaris_10/Product
```

Instalando os programas:
{: style="text-align: justify;"}

```console
-bash-3.2# pkgadd -d . SUNWsshr
-bash-3.2# pkgadd -d . SUNWsshu
```

Verifique se foram instalados com sucesso:
{: style="text-align: justify;"}

```console
-bash-3.2# pkginfo | grep ssh

system      SUNWsshcu                    SSH Common, (Usr)
system      SUNWsshdr                    SSH Server, (Root)
system      SUNWsshdu                    SSH Server, (Usr)
system      SUNWsshr                     SSH Client and utilities, (Root)
system      SUNWsshu                     SSH Client and utilities, (Usr)
```

**Dica:** Ainda resta a última opção utilizar arquivos .pkg gerados previamente. Veja esse artigo *[Solaris 10 - Como criar pacotes .pkg](https://cr-signorelli.github.io/solaris-10/solaris-10-como-criar-pacotes-pkg/)* que contém um passo simples de como criar um pacote de instalação, dessa forma será possível transferir apenas dois pacotes pequenos ao invés de uma imagem .ISO de mais de 2GB.
{: .notice--info}
