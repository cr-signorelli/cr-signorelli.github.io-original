---
title: "Solaris 10 - Como criar tunel IP"
classes: wide
permalink: /solaris-10-como-criar-tunel-ip/
header:
  overlay_image: https://source.unsplash.com/fdvTTpkAKkY/1920x300
  og_image: https://source.unsplash.com/fdvTTpkAKkY/120x120
  teaser: https://source.unsplash.com/fdvTTpkAKkY/200x100
  image_description: "Foto dentro de um túnel"
  caption: "Imagem: [**bantersnaps**](https://unsplash.com/photos/fdvTTpkAKkY)"
  #actions:
  #  - label: "Leia mais"
  #    url: "https://cr-signorelli.github.io/"
excerpt: "Os túneis IP fornecem um meio de transportar pacotes de dados entre domínios de broadcast."
categories:
  - solaris-10
tags:
  - solaris-10
  - sparc
  - sysadmin
  - ip-tunnel
  - network
show_date: false
date: 2021-05-02T17:30:00-03:00
last_modified_at: 2021-05-02T17:30:00-03:00
---

Especificações que vamos usar para o exemplo:  
{: style="text-align: justify;"}

> 10.137.0.0/17 = rede que precisamos alcançar em outra máquina  
> 192.168.70.1/30 = outro lado do tunel IP  
> 192.159.205.12 = IP do outro lado  
> 192.168.70.2/30 = nosso lado do tunel IP  
> 192.234.0.23 = IP do nosso lado  

Antes de criar o tunel vamos analisar a sintaxe do comandos.
{: style="text-align: justify;"}

Estrutura do comando **ifconfig** para criar rede:
{: style="text-align: justify;"}

```console
-bash-3.2# ifconfig ip.tun0 <IP_TUNNEL_SRC> <IP_TUNNEL_DST> tsrc <IP_INTERFACE_SRC> tdst <IP_INTERFACE_DST> up
```

Analisando as opções:
{: style="text-align: justify;"}

> \<IP_TUNNEL_SRC\> - IP de origem do tunel.  
> \<IP_TUNNEL_DST\> - IP de destino do tunel.  
> \<IP_INTERFACE_SRC\> - IP de origem da interface que vamos usar para encapsular o tráfego.  
> \<IP_INTERFACE_DST\> - IP de destino da interface que vamos usar para desencapsular o tráfego.  

Estrutura do comando **route**:
{: style="text-align: justify;"}

```console
-bash-3.2# route add <NETWORK_ON_TUNNER> <IP_TUNNEL_DST>
```

Analisando as opções:  
{: style="text-align: justify;"}

> \<NETWORK_ON_TUNNER\> - IP da rede que deseja acessar através do tunel.  
> \<IP_TUNNEL_DST\> - IP do tunel de destino.  

Estrutura do firewall, será preciso adicionar algumas regras no **ipf.conf**
{: style="text-align: justify;"}

```console
pass in from <IP_INTERFACE_DST> to any
pass out from any to <IP_INTERFACE_DST>
```

Analisando as opções:
{: style="text-align: justify;"}

> \<IP_INTERFACE_DST\> - IP de destino da interface que vamos usar para desencapsular o trafego.  

Criando o tunel e adicionando as rotas:
{: style="text-align: justify;"}

```console
-bash-3.2# ifconfig ip.tun0 plumb 
-bash-3.2# ifconfig ip.tun0 192.168.70.2 192.168.70.1 tsrc 192.234.0.23 tdst 192.159.205.12 up
-bash-3.2# route add 10.137.0.0/17 192.168.70.1
```

Adicione as regras do firewall
{: style="text-align: justify;"}

```console
pass in from 192.159.205.12 to any
pass out from any to 192.159.205.12
```

#### Referências

> [Oracle Solaris Administration: IP Services](http://docs.oracle.com/cd/E19253-01/816-5166/6mbb1kq31/){:target="_blank"}  
