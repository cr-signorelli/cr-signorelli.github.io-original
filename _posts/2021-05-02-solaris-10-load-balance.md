---
title: "Solaris 10 - Load Balance"
classes: wide
header:
  overlay_image: /assets/images/solaris10.jpg
  og_image: /assets/images/solaris10-og.jpg
  teaser: /assets/images/solaris10-thumb.jpg
  image_description: "Fundo azulado com desenho de meio sol"
  #caption: "Imagem: [signorelli](https://pixabay.com/)"
  #actions:
  #  - label: "Leia mais"
  #    url: "https://cr-signorelli.github.io/"
categories:
  - solaris-10
tags:
  - solaris-10
  - sparc
  - sysadmin
  - network
  - load-balance
show_date: false
date: 2021-05-02T17:30:00-03:00
last_modified_at: 2021-05-02T17:30:00-03:00
---

Load Balance é uma ferramenta muito útil para ajudar a distribuir e balancear o trafego entre diferentes servidores.

Existem inúmeras formas de montar um load balance via software ou utilizando um hardware dedicado, mas em algumas situações é preciso trabalhar com que temos disponível ou quando estamos em uma fase conceitual de uma solução ou até mesmo realizando uma prova de conceito.

Servidores com processado SPARC não comuns de serrem encontrados no mercado, é um nicho um pouco restrito conseguir acesso a esse tipo de equipamento. Embora tenha diversas vantagens trabalhar com tecnologia SPARC eleva demais o desafio principalmente com compatibilidade se softwares em geral.

Por isso nesse exemplo quero mostrar como montar um load balance utilizando um Servidor Oracle com processador SPARC com Solaris 10 instalado utilizando OVM (logical domains).

#### Informações

> Laboratório realizado em um Oracle T3-1 com Solaris 10.  
> Para virtualização das máquinas foi usando OVM (Logical Domains).  
> 3 máquinas virtuais (LDOMs) já pré configurados, também utilizando Solaris 10.
> IP Filter usado como balance.  

Especificações que vamos usar para o exemplo:  
{: style="text-align: justify;"}

> 192.168.100.0/24 = será nossa rede interna  
> 198.51.100.0/24 = será nossa rede válida, ou seja, nosso acesso para internet  
> 198.51.100.116 = IP da Primary (será nosso IP de saida para internet)
> igb0 = inteface rede da Primary

#### Primary - configuração da rede + ipf + ipnat

Vamos utilizar a Primary, ela será o balance. Para quem não estiver familiarizado com o termo "Primary" é a máquina física principal que está com OVM instalado.

Atribua um ip invalido que vai funcionar como gw para as máquinas virtuais, tem que colocar o ip na VSW0 (switch virtual)

```console
-bash-3.2# ifconfig vsw0 plumb
-bash-3.2# ifconfig vsw0 192.168.100.1 netmask 255.255.255.0 up
```

Libere o encaminhamento (forwarding):

```console
-bash-3.2# /usr/sbin/ndd -set /dev/ip ip_forwarding 1
```

Para simplificar a configuração e os testes iniciais vamos reconfigurar o firewall deixando o mínimo possível de regras. Não desligue o firewall vamos precisar dele para o balance. Edite o arquivo **/etc/ipf/ipf.conf**

```console
-bash-3.2# vi /etc/ipf/ipf.conf
```

Conteúdo do arquivo **/etc/ipf/ipf.conf**

```console
pass in all
pass out all
```

Vamos criar o arquivo IPNAT.CONF, aqui vamos usar 3 tecnicas juntas:

1. Criar um proxy transparente para navegacao da LDOM  
2. Criar o encaminhamento da portas  
3. Dentro do encaminhamento de portas vamos fazer o round-robin  

```console
-bash-3.2# vi /etc/ipf/ipnat.conf
```

Conteúdo do arquivo **/etc/ipf/ipnat.conf**

```console
# Joga as requisicoes do APACHE na fila
rdr igb0 198.51.100.116 port 80 -> 192.168.100.2  port 80 tcp round-robin
rdr igb0 198.51.100.116 port 80 -> 192.168.100.3  port 80 tcp round-robin
rdr igb0 198.51.100.116 port 80 -> 192.168.100.4  port 80 tcp round-robin

# Garante que as respostas do APACHE das maquinas serao recebidas de volta
map igb0 from any to 192.168.100.2  port = 80 -> 198.51.100.116
map igb0 from any to 192.168.100.3  port = 80 -> 198.51.100.116
map igb0 from any to 192.168.100.4  port = 80 -> 198.51.100.116

# Garante que as respostas em geral das maquinas serao recebidas de volta
map igb0 192.168.100.2  -> 198.51.100.116
map igb0 192.168.100.3  -> 198.51.100.116
map igb0 192.168.100.4  -> 198.51.100.116
```

Para facilitar o processo de iniciar o ipf + ipnat vamos criar um script **reload-firewall.sh**, e só para garantir coloquei o forwarding novamente:

```console
-bash-3.2# cd /etc/ipf/
-bash-3.2# vi /etc/ipf/reload-firewall.sh
```

Conteúdo do arquivo **/etc/ipf/reload-firewall.sh**

```console
ipf -D
ipf -E
ipf -Fa
ipnat -FC
ippool -F
ippool -f /etc/ipf/ippool.conf
ipf -Fa -f /etc/ipf/ipf.conf
ipnat -FC -f /etc/ipf/ipnat.conf
/usr/sbin/ndd -set /dev/ip ip_forwarding 1
ipfstat -hio
```

Para iniciar o firewall automaticamente crie o arquivo **/etc/rc2.d/S99firewall**

```console
-bash-3.2# vi /etc/rc2.d/S99firewall
```

Conteúdo do arquivo **/etc/rc2.d/S99firewall**

```console
ipf -D
sleep 3
ipf -E
sleep 3
ipf -Fa
sleep 3
ipnat -FC
sleep 3
ippool -F
ippool -f /etc/ipf/ippool.conf
ipf -Fa -f /etc/ipf/ipf.conf
ipnat -FC -f /etc/ipf/ipnat.conf
/usr/sbin/ndd -set /dev/ip ip_forwarding 1
```

Não esqueça de ajustar a permissão de execução dos arquivos criados:

```console
-bash-3.2# chmod +x /etc/ipf/reload-firewall.sh
-bash-3.2# chmod +x /etc/rc2.d/S99firewall
```

**Alerta:** Quando finalizar suas configurações e testes com o load balance, carregue suas regras de firewall para não ficar sem proteção.
{: .notice--danger}

#### Primary - estrutura da LDOM's

Antes de continuar vamos analisar dois cenários de configurar a forma como as máquinas virtuais (LDOM's) irão conversar com a Primary + Load Balance. Particularmente o Cenário 2 é muito mais vantajoso mais tem a questão do seu hardware ser ou não compatível com essa solução.

**Cenário 1** - Utilizar a mesma placa de rede física, na prática realmente usamos o mesmo VIRTUAL-SWITCH para sair para pela rede

```console
                          -----------------
                          | Placa de Rede |
                          -----------------
                                  |
    -----------              -----------
    | LDOM    |              | Primary |
    -----------              -----------
        |                  /             \
        |                  |              |
        |                  |              |
     ---------        -------            --------
     | vnet0 |        | igb0 |           | vsw0 |
     ---------        -------            --------
        /\                 |              |
         |                 |              |
         |                ------------------
         |--------------> |   switch - vsw |
    (todos PACOTES)       ------------------
```

**Cenário 2** - Descobri algo interessante do servidor da SPARC (a partir da SPARC-T2), é possivel habilitar um recurso "híbrido" nas interfaces vnet. Esse modo híbrido usa um recurso chamada DMA, só funciona até 3 vnet, se colocar mais não tem problema, mas só vai funcionar nas 3 primeiras configuradas. Outra coisa interessante é que o modo híbrido é uma sugestão, ou seja, se a placa não tem suporte não atrapalha em nada o funcionamento.

DMA - separa os pacotes em UNICAST e BROADCAST, com isso o UNICAST vai direto para placa de rede física e o BROADCAST esse passa por dentro da primary.

```console
                           -----------------
                    |----> | Placa de Rede |
        (UNICAST)   |      -----------------
                    |             |
    -----------     |        -----------
    | LDOM    | <---|        | Primary |
    -----------              -----------
        |                  /             \
        |                  |              |
        |                  |              |
     ---------        -------            --------
     | vnet0 |        | igb0 |           | vsw0 |
     ---------        -------            --------
        /\                 |              |
         |                 |              |
         |  (BROADCAST)   ------------------
         |--------------> |   switch - vsw |
                          ------------------
```

#### Primary - Cenário 1 - configurando as placa de redes

Vamos adicionar uma nova interface de rede virtual a VNET1 para nao mexer na VNET0 só por segurança.

Sem o modo híbrido:

```console
-bash-3.2# ldm add-vnet vnet1 primary-vsw0 ldom1
-bash-3.2# ldm add-vnet vnet1 primary-vsw0 ldom2
-bash-3.2# ldm add-vnet vnet1 primary-vsw0 ldom3
```

Por segurança eu reiniciei as LDOM

```console
-bash-3.2# ldm stop-domain ldom1
-bash-3.2# ldm stop-domain ldom2
-bash-3.2# ldm stop-domain ldom3

-bash-3.2# ldm start-domain ldom1
-bash-3.2# ldm start-domain ldom2
-bash-3.2# ldm start-domain ldom3
```

Para ver ser está tudo ok, olhe na parte de NETWORK:

```console
-bash-3.2# ldm list -l ldom1
```

Caso seja preciso remover a VNET da LDOM:

```console
-bash-3.2# ldm rm-vnet vnet1 ldom1
-bash-3.2# ldm rm-vnet vnet1 ldom2
-bash-3.2# ldm rm-vnet vnet1 ldom3
```

#### Primary - Cenário 2 - configurando as placa de redes

Com o modo híbrido:

```console
-bash-3.2# ldm add-vnet mode=hybrid vnet1 primary-vsw0 ldom1
-bash-3.2# ldm add-vnet mode=hybrid vnet1 primary-vsw0 ldom2
-bash-3.2# ldm add-vnet mode=hybrid vnet1 primary-vsw0 ldom3
```

Por segurança eu reiniciei as LDOM:

```console
-bash-3.2# ldm stop-domain ldom1
-bash-3.2# ldm stop-domain ldom2
-bash-3.2# ldm stop-domain ldom3

-bash-3.2# ldm start-domain ldom1
-bash-3.2# ldm start-domain ldom2
-bash-3.2# ldm start-domain ldom3
```

Para ver ser está tudo ok, olha na parte de NETWORK:

```console
-bash-3.2# ldm list -l ldom1
```

Caso seja preciso remover a VNET da LDOM:

```console
-bash-3.2# ldm rm-vnet vnet1 ldom1
-bash-3.2# ldm rm-vnet vnet1 ldom2
-bash-3.2# ldm rm-vnet vnet1 ldom3
```

Caso seja preciso só deligar o modo híbrido:

```console
-bash-3.2# ldm set-vnet mode= vnet1 ldom1
-bash-3.2# ldm set-vnet mode= vnet1 ldom2
-bash-3.2# ldm set-vnet mode= vnet1 ldom3
```

---

#### LDOM 1 - configurando a rede dentro da maquina virtual

```console
-bash-3.2# ifconfig vnet1 plumb
-bash-3.2# ifconfig vnet1 192.168.100.2 netmask 255.255.255.0 up
-bash-3.2# route add default 192.168.100.1
-bash-3.2# route delete default 198.51.100.1
-bash-3.2# ifconfig vnet0 down
```

#### LDOM 2 - configurando a rede dentro da maquina virtual

```console
-bash-3.2# ifconfig vnet1 plumb
-bash-3.2# ifconfig vnet1 192.168.100.3 netmask 255.255.255.0 up
-bash-3.2# route add default 192.168.100.1
-bash-3.2# route delete default 198.51.100.1
-bash-3.2# ifconfig vnet0 down
```

#### LDOM 3 - configurando a rede dentro da maquina virtual

```console
-bash-3.2# ifconfig vnet1 plumb
-bash-3.2# ifconfig vnet1 192.168.100.4 netmask 255.255.255.0 up
-bash-3.2# route add default 192.168.100.1
-bash-3.2# route delete default 198.51.100.1
-bash-3.2# ifconfig vnet0 down
```

#### Troubleshooting

Mostrar p ipf + ippool carregados:

```console
-bash-3.2# ipfstat -hio
```

Estatísticas do firewall, similar ao top no linux:

```console
-bash-3.2# ipfstat -t
```

Mostra os MAP/Redirect do IPNAT que estão sendo realizados:

```console
-bash-3.2# ipnat -l
```

**Alerta:** Round-Robin que está sendo usado nessa solução não possui nenhum "HEALTH-CHECK", ou seja, se uma das máquina cair ou a aplicação travar o balance vai continuar a mandar o tráfego para lá, será preciso acessar o arquivo de IPNAT remover a máquina da lista e recarregar o IPF + IPNAT.
{: .notice--danger}