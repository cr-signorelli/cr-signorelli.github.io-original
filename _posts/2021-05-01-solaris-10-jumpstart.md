---
title: "Solaris 10 - JumpStart"
classes: wide
header:
  overlay_image: /assets/images/solaris10-optimize.jpg
  og_image: /assets/images/solaris10-optimize-og.jpg
  teaser: /assets/images/solaris10-optimize-thumb.jpg
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
  - network
  - instalacao
last_modified_at: 2021-05-01T23:00:00-03:00
---

É muito comum que servidores não possuam unidade CD/DVD, algumas modelos da Oracle não possuem nem entrada USB, ou seja, a única fora de instalar o sistema é via rede.
{: style="text-align: justify;"}

#### Requisitos

> Imagem .ISO do Solaris 10 64bits SPARC.  
> Outra máquina com Solaris 10 instalado (vamos utilizar um máquina virtual usando VirtuaBox).  
> Configure a interface do VirtualBox como "bridge".  
> Configura a máquina virtual na mesma subnet do servidor onde o Solaris 10 será instalado.  
> Monte a imagem .ISO no VirtualBox e adicione a sua máquina virtual.  

Ao adicionar a imagem .ISO via VirtualBox o Solaris irá idenficá-la como uma unidade de CD/DVD.
{: style="text-align: justify;"}

Acesse o sistema como **root** a unidade de CD/DVD deve será montada automaticamente pelo serviço **volfs**
{: style="text-align: justify;"}

Instalando o JumpStart server:
{: style="text-align: justify;"}

```console
-bash-3.2# mkdir -p /path/to/anywhere/sol
-bash-3.2# mkdir -p /path/to/anywhere/config
-bash-3.2# cd /cdrom/cdrom0/S0/Solaris_10/Tools
-bash-3.2# ./setup_install_server /path/to/anywhere
```

Adicione os seguinte parametros no arquivo **/etc/dfs/dfstab** para compartilhar o diretório:
{: style="text-align: justify;"}

```console
share -F nfs -o ro,anon=0 -d "install dir" /path/to/anywhere/sol
```

Execute o comando **shareall** e  **share** e verifique se o diretório foi compartilhados:
{: style="text-align: justify;"}

```console
-bash-3.2# shareall
-bash-3.2# share
```

Certifique se o serviço de NFSD esta rodando:
{: style="text-align: justify;"}

```console
-bash-3.2# svcs -l svc:/network/nfs/server:default
```

Caso esteja desligado execute o comando a seguir
{: style="text-align: justify;"}

```console
-bash-3.2# svcadm enable svc:/network/nfs/server:default
```

Configurando o JumpStart:
{: style="text-align: justify;"}

```console
-bash-3.2# cd /cdrom/cdrom0/S0/Solaris_10/Tools
-bash-3.2# ./setup_install_server -b /path/to/boot
-bash-3.2# cp /cdrom/cdrom0/s0/Solaris_10/Misc/jumpstart_sample/* /path/to/config
```

Parâmetros para regras, edite o arquivo **/path/to/config/rules** e adicione os seguintes dados:
{: style="text-align: justify;"}

```console
network net_ip.0 && arch sparc – profile -
```

Edite o arquivo **profile** e adicione os seguintes dados:
{: style="text-align: justify;"}

```console
install_type initial_install / system_type standalone
```

Edite o arquivo **sysidcfg** e adicione as linhas padrões referente as configurações da máquina que será instalada. Caso tenha alguma duvida consulte a [documentação oficial](https://docs.oracle.com/cd/E26505_01/html/E28037/preconsysid-55534.html).
{: style="text-align: justify;"}

Exemplo do arquivo **sysidcfg** para SPARC:
{: style="text-align: justify;"}

```console
keyboard=US-English
system_locale=en_US
timezone=US/Central
terminal=sun-cmd
timeserver=localhost
name_service=NIS {domain_name=marquee.central.example.com
                  name_server=nmsvr2(172.31.112.3)}
nfs4_domain=dynamic
root_password=m4QPOWNY
network_interface=hme0 {hostname=host1 
                       default_route=172.31.88.1 
                       ip_address=172.31.88.210 
                       netmask=255.255.0.0 
                       protocol_ipv6=no}
security_policy=kerberos {default_realm=example.com 
                          admin_server=krbadmin.example.com 
                          kdc=kdc1.example.com, 
                          kdc2.example.com}
```

Exemplo do arquivo **sysidcfg** para x86:
{: style="text-align: justify;"}

```console
keyboard=US-English
timezone=US/Central
timeserver=timehost1
terminal=ibm-pc
service_profile=limited_net

name_service=NIS {domain_name=marquee.central.example.com
                  name_server=nmsvr2(172.25.112.3)}
nfs4_domain=example.com
root_password=URFUni9
```

Desmontando a o CD/DVD:
{: style="text-align: justify;"}

```console
-bash-3.2# cd /
-bash-3.2# umount the DVD
```

Especificações que vamos usar para o exemplo:
{: style="text-align: justify;"}

> server01 = hostname  
> 192.0.2.10/24 = endereço IP  
> 3:22:11:6d:2e:1f = macaddress do servidor  

Edite o arquivo **/etc/hosts** e adicione o hostname e o IP do servidor que será instalado, exemplo:
{: style="text-align: justify;"}

```console
192.0.2.10 server01
```

Adicionado o servidor na JumpStart:
{: style="text-align: justify;"}

```console
-bash-3.2# cd /path/to/anywhere/Solaris_10/Tools
-bash-3.2# ./add_install_client -e 3:22:11:6d:2e:1f -s 192.0.2.10:/path/to/anywhere/sol -c 192.0.2.10:/path/to/anywhere/config -p 192.0.2.10:/path/to/anywhere/config server01 sun4v
```

Acesso o arquivo **/etc/bootparams** e verifique o parâmetro **root_server** não é localhost, tem que ser o IP da sua máquina virtual
{: style="text-align: justify;"}

Agora acesse o ALOM ou ILOM do seu servidor que irá receber a instalação vá até o **prompt ok** e inicie a instalação:
{: style="text-align: justify;"}

```console
boot net -v – install
```

Ao iniciar a instalação siga as instruções na tela.
{: style="text-align: justify;"}

#### Referências

> [Planning to Install Over the Network](https://docs.oracle.com/cd/E26505_01/html/E28037/ejusv.html#scrolltoc)  
> [Documentação sobre sysidcfg](https://docs.oracle.com/cd/E26505_01/html/E28037/preconsysid-55534.html)  
