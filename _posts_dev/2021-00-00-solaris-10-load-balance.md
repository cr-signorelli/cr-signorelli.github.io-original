---
title: "Solaris 10 - Procedimento básico de instalação"
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
last_modified_at: 2021-04-22T23:00:00-03:00
---

# Solaris 10 - Load Balance

---

**Primary**

Para o exemplo a seguir devemos considerar algumas informações sobre os IP da rede.
* 192.168.100.0/24 - será nossa rede interna
* 198.51.100.0/24 - será nossa rede válida, ou seja, nosso acesso para internet 


1. Atribuir um ip invalido que vai funcionar como gw para as maquinas virtuais, tem que colocar o ip na VSW0 --> o switch virtual
```console
    ifconfig vsw0 plumb
    ifconfig vsw0 192.168.100.1 netmask 255.255.255.0 up
```

2. Liberar o encaminhamento
```console
/usr/sbin/ndd -set /dev/ip ip_forwarding 1
```

3. Arrumar o IPF.CONF, (para fazer os testes esta passando tudo e aceitando tudo)
```console
vi /etc/ipf/ipf.conf
    pass in all
    pass out all
```

4. Editar o IPNAT.CONF, aqui vamos usar 3 tecnicas juntas
* criar um proxy transparente para navegacao da LDOM
* criar o encaminhamento da portas
* dentro do encaminhamento de portas vamos fazer o round-robin

```console
vi /etc/ipf/ipnat.conf
    # Joga as requisicoes para DNS na fila
    rdr igb0 198.51.100.116 port 53 -> 192.168.100.2 port 53 udp round-robin
    # Exemplo usando Apache
    rdr igb0 198.51.100.116 port 80 -> 192.168.100.2  port 80 tcp round-robin
    # Garante que as respostas do DNS das maquinas serao recebidas de volta
    map igb0 from any to 192.168.100.2  port = 53 -> 198.51.100.116
    # Exemplo usando Apache
    map igb0 from any to 192.168.100.2  port = 80 -> 198.51.100.116
    # Garante que as respostas em geral das maquinas serao recebidas de volta
    map igb0 192.168.100.2  -> 198.51.100.116
```

5. Como ligar o ipf e o ipnat, só para garantir coloquei o forwarding novamente
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

Como habilitar ou reabilitar o ipnat individualmente: ipnat -FC -f /etc/ipf/ipnat.conf

6. Como iniciar o firewall automaticamente
```console
vim /etc/rc2.d/S99firewall
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

**Primary - Estrutura**

Mexendo na estrutura das maquinas virtuais LDOM

* Comentário: sem o IP na VSW0 nao tem comunicacao da PRIMARY com a LDOM, tentei colocar o IP em um interface virtual "IGB0:1" nao funcionou
* Comentário: mesmo com a tatica do VSW0 nao tenho certeza de que a trafego nao está indo até o router !!!!
* Comentário: nao tem logica fazer a LDOM ir a até ROUTER e voltar para PRIMRY, temos que confinar a comunicao delas no PRIMARY !!!!
* Comentário: ja que a ideia é criar um balance, ou seja afunilar as requisições na PRIMARY e ela distribuir para LDOM, resolvi compartilhar a mesma interface fisica para todas as maquinas.

OBS.: lembrando que a maquina virtual terá um IP "invalido" então de qualquer forma terá que usar o "PROXY transparente" para chegar na internet

Cenario1 - Utilizar a mesma placa de rede fisica, na pratica realmente usamos o mesmo VIRTUAL-SWITCH para sair para pela rede
```
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

Cenario2 - Descobri algo interessante do servidor da SUN (a partir da SPARC-T2), é possivel habilitar um recurso "hibrido" nas interfaces vnet0. Esse modo hibrido usa um recurso chamada DMA, só funciona até 3 vnet, se colocar mais nao tem problema, mas só vai funcionar nas 3 primeiras configuradas. Outra coisa interessante é que o modo hibrido é uma sugestão, ou seja se a placa nao tem supporte nao atrapalha em nada.

DMA - ele separa os pacotes em UNICAST e BROADCAST:
* UNICAST vai direto para placa de rede fisica
* BROADCAST esse passa por dentro da primary
```
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

**Primary - Configurando as placa de redes**

Vou criar a VNET1 para nao mexer na VNET0 só por segurança

**Cenario 1**

Sem o modo hibrido
```console
ldm add-vnet vnet1 primary-vsw0 ldom1
ldm add-vnet vnet1 primary-vsw0 ldom2
ldm add-vnet vnet1 primary-vsw0 ldom3
```

Por segurança eu reiniciei as LDOM
```console
ldm stop-domain ldom1
ldm stop-domain ldom2
ldm stop-domain ldom3
ldm start-domain ldom1
ldm start-domain ldom2
ldm start-domain ldom3
```

Para ver ser está tudo ok, olhe na parte de "NETWORK"
```console
ldm list -l ldom1
```

Caso seja preciso remover a VNET da LDOM
```console
ldm rm-vnet vnet1 ldom1
ldm rm-vnet vnet1 ldom2
ldm rm-vnet vnet1 ldom3
```

**Cenario 2**

Com o modo hibrido
```console
ldm add-vnet mode=hybrid vnet1 primary-vsw0 ldom1
ldm add-vnet mode=hybrid vnet1 primary-vsw0 ldom2
ldm add-vnet mode=hybrid vnet1 primary-vsw0 ldom3
```

Por segurança eu reiniciei as LDOM
```console
ldm stop-domain ldom1
ldm stop-domain ldom2
ldm stop-domain ldom3
ldm start-domain ldom1
ldm start-domain ldom2
ldm start-domain ldom3
```

Para ver ser está tudo ok, olha na parte de "NETWORK"
```console
ldm list -l ldom1
```

Caso seja preciso remover a VNET da LDOM
```console
ldm rm-vnet vnet1 ldom1
ldm rm-vnet vnet1 ldom2
ldm rm-vnet vnet1 ldom3
```

Caso seja preciso só deligar o modo hibrido
```console
ldm set-vnet mode= vnet1 ldom1
ldm set-vnet mode= vnet1 ldom2
ldm set-vnet mode= vnet1 ldom3
```

---

**LDOM - Configurando a rede(apenas de forma temporaria)**

Habilitar a VNET1
```console
ifconfig vnet1 plumb
ifconfig vnet1 192.168.100.2 netmask 255.255.255.0 up
```

Adicionando o DEFAULT GW para jogar a saida para o Proxy transparente
```console
route add default 192.168.100.1
```

Removendo o antigoGW
```console
route delete default 198.51.100.1
```

Desligando a VNET0
```console
ifconfig vnet0 down
```

---

**Troubleshooting**

Mostrar p ipf + ippool carregados
```console
ipfstat -hio
```

Statisticas do firewall, similar ao top no linux
```console
ipfstat -t
```

Mostra os MAP/Redirect do IPNAT que estão sendo realizados
```console
ipnat -l
```

---

**Problemas**

ATENÇÃO - Round-Robin é burro, ou seja ele nao faz analise nenhum, simplesmente jogo um solicitação/pacotes para cada um na lista em sequencia

PROBLEMA.1 - Falha no Round-Robin, se uma das maquinas cai ela continua na lista, o IPNAT continua enviando pacotes para ela.
* Pratica: A resolução de DNS comeca a ter falhas, quando for a vez da LDOM ser consultada no Round-Robin vc terá falha na exibição da pagina. Depois que der "pagina nao pode ser exibida" basta apertar F5 e quando a consulta for jogada em outra LDOM pelo Round-Robin a pagina abre.

PROBLEMA.1.1 - A Falha relatada no PROBLEMA.1 "nao tem cura" ela pode se repitar a cada resolução de nome, o unico modo que eu encontrei foi remover do IPNAT e recarregar

SOLUÇÃO DO PROBLEMA.1 : é possivel resolver com um SCRIPT-SHELL e verificar a resolução de nome de cada LDOM e alguma falhar ele remove do IPNAT e recarrega o servico