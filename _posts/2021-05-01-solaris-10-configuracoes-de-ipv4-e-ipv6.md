---
title: "Solaris 10 - Configurações de IPv4 e Ipv6"
classes: wide
permalink: /solaris-10-configuracoes-de-ipv4-e-ipv6/
header:
  overlay_image: https://source.unsplash.com/wQLAGv4_OYs/1920x300
  og_image: https://source.unsplash.com/wQLAGv4_OYs/120x120
  teaser: https://source.unsplash.com/wQLAGv4_OYs/200x100
  image_description: "Fundo abstrato colorido"
  caption: "Imagem: [**Lucas Benjamin**](https://unsplash.com/photos/wQLAGv4_OYs)"
  #actions:
  #  - label: "Leia mais"
  #    url: "https://cr-signorelli.github.io/"
excerpt: "Quando lidamos com sistema operacinais legados é normal encontrar a interface IPv6 desabilitada. Veja como habilitá-la."
categories:
  - solaris-10
tags:
  - solaris-10
  - sparc
  - sysadmin
  - network
show_date: false
date: 2021-05-01T20:00:00-03:00
last_modified_at: 2021-05-01T20:00:00-03:00
---

Configurar a interface de IPv4 ou IPv6 manualmente não é algo complicado, mas é importante fixar nos dados e arquivos necessários para não perder as configurações ao reiniciar o sistema ou a serviço de rede.
{: style="text-align: justify;"}

#### IPv4 interface física

Especificações que vamos usar para o exemplo:
{: style="text-align: justify;"}

> igb0 = physical network interface  
> 192.0.2.10/24 = interface ip address  
> 192.0.2.1 = default gateway  

Use o comando **ifconfig** para habilitar a interface de rede primeiro:
{: style="text-align: justify;"}

```console
-bash-3.2# ifconfig igb0 plumb
```

Verifique se a placa foi ativada e pronta para receber as configurações de IP e máscara:
{: style="text-align: justify;"}

```console
-bash-3.2# ifconfig -a

igb0: flags=842 mtu 1500
inet 0.0.0.0 netmask 0
ether 3:22:11:6d:2e:1f
```

Configurando o IP, máscara de rede e ativando a placa de rede:
{: style="text-align: justify;"}

```console
-bash-3.2# ifconfig igb0 192.0.2.10 netmask 255.255.255.0 up
```

Verifique novamente as configurações para certifica-se que foram aplicadas corretamente:
{: style="text-align: justify;"}

```console
-bash-3.2# ifconfig -a

igb0: flags=843 mtu 1500
inet 192.0.2.10 netmask ffffff00 broadcast 192.9.2.255 ether 3:22:11:6d:2e:1f
```

#### IPv4 interface virtual

A interface virtual pode ser configurada para permitir que placa igb0 receba mais de um endereço IP:
{: style="text-align: justify;"}

```console
-bash-3.2# ifconfig igb0:1 198.51.100.10 netmask 255.255.255.0 up
```

Verifique se a interface virtual foi configurada corretamente:
{: style="text-align: justify;"}

```console
-bash-3.2# ifconfig -a

igb0: flags=843 mtu 1500
inet 192.0.2.10 netmask ffffff00 broadcast 192.9.2.255 ether 3:22:11:6d:2e:1f
igb0:1: flags=842 mtu 1500
inet 198.51.100.10 netmask ffffff00 broadcast 172.40.255.255
```

Adicionando a interface na inicialização do sistema:
{: style="text-align: justify;"}

```console
-bash-3.2# echo "192.0.2.10" > /etc/hostname.igb0
```

Ajuste de permissões do arquivo:
{: style="text-align: justify;"}

```console
-bash-3.2# chmod 644 /etc/hostname.igb0
```

#### IPV4 Rotas

Adicione a rota padrão (default) de fora permanete:
{: style="text-align: justify;"}

```console
-bash-3.2# route -p add default 192.0.2.2 1
```

Caso precisa alterar a rota:
{: style="text-align: justify;"}

```console
-bash-3.2# route change default 192.0.2.1 1
```

Adicionando um rota espeficida da rede 10.0.0.0/8 para um 198.51.100.1 de outra rede:
{: style="text-align: justify;"}

```console
-bash-3.2# route add -net 10.0.0.0 -netmask 255.0.0.0 198.51.100.1 1
```

#### IP-Forwarding

IP-Forwarding permite que você encaminhe todas as solicitações provenientes de uma determinada porta ou URL para serem um endereço IP especificado.
{: style="text-align: justify;"}

Padrão esse configurações vem desativada use o comando **ndd** para ligar essa opção:
{: style="text-align: justify;"}

```console
-bash-3.2# ndd -set /dev/ip ip_forwarding 0
```

Para que o IP forwarding seja iniciado automaticamente junto do sistema operacional adicione no boot do sistema, arquivo responsável por isso é **/etc/rc2.d/S69inet**.
{: style="text-align: justify;"}

#### IPv6 interface física

Especificações que vamos usar para o exemplo:
{: style="text-align: justify;"}

> igb0 = physical network interface  
> 2001:db8:1:0:0:0:0:100/128 = interface ip address  
> 2001:db8:1:0:0:0:0:1 = default gateway  

Habilitando a interface:
{: style="text-align: justify;"}

```console
-bash-3.2# ifconfig inet6 igb0 plumb up
```

Configurando IP na interface:
{: style="text-align: justify;"}

```console
-bash-3.2# ifconfig igb0 inet6 addif 2001:db8:1:0:0:0:0:100/128 up
```

Adicionando a rota padrão (default route) de forma permanente:
{: style="text-align: justify;"}

```console
-bash-3.2# route -p add -inet6 default 2001:db8:1:0:0:0:0:1
```

Adicionando a interface na inicialização do sistema:
{: style="text-align: justify;"}

```console
-bash-3.2# echo "addif 2001:db8:1:0:0:0:0:100/128 up" > /etc/hostname6.igb0
```

Ajuste de permissões do arquivo:
{: style="text-align: justify;"}

```console
-bash-3.2# chmod 644 /etc/hostname6.igb0
```
