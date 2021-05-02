---
title: "Solaris 10 - Como criar tunel IP"
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
  - ip-tunnel
  - network
last_modified_at: 2021-05-02T17:30:00-03:00
---

Os túneis IP fornecem um meio de transportar pacotes de dados entre domínios de broadcast.
{: style="text-align: justify;"}

**Especificações que vamos usar para o exemplo:**  

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

> **Analisando as opções:**  
>  
> \<IP_TUNNEL_SRC\> - IP de origem do tunel (Non-routing IP of the source tunnel)  
> \<IP_TUNNEL_DST\> - IP de destino do tunel (Non-routing IP of the destination tunnel)  
> \<IP_INTERFACE_SRC\> - IP de origem da interface que vamos usar para encapsular o tráfego (IP from source interface which we use to encapsulate the traffic)  
> \<IP_INTERFACE_DST\> - IP de destino da interface que vamos usar para desencapsular o tráfego (IP from destination interface which we use to unencapsulate the traffic)  

Estrutura do comando **route**:
{: style="text-align: justify;"}

```console
-bash-3.2# route add <NETWORK_ON_TUNNER> <IP_TUNNEL_DST>
```

> **Analisando as opções:**  
>  
> \<NETWORK_ON_TUNNER\> - IP da rede que deseja acessar através do tunel (IP address of the network you want to access through the tunnel)  
> \<IP_TUNNEL_DST\> - IP do tunel de destino (Non-routing IP of the destination tunnel)  

Estrutura do firewall, será preciso adicionar algumas regras no **ipf.conf**
{: style="text-align: justify;"}

```console
pass in from <IP_INTERFACE_DST> to any
pass out from any to <IP_INTERFACE_DST>
```

> **Analisando as opções:**  
>  
> \<IP_INTERFACE_DST\> - IP de destino da interface que vamos usar para desencapsular o trafego (IP from destination interface which we use to unencapsulate the traffic)  

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

> [Oracle Solaris Administration: IP Services](http://docs.oracle.com/cd/E19253-01/816-5166/6mbb1kq31/)  
