---
title: "Solaris 10 - Procedimento básico de instalação"
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
date: 2021-04-22T23:00:00-03:00
last_modified_at: 2021-04-22T23:00:00-03:00
---

# Solaris 10 - SPARC

**Dicas e comandos úteis.**

---

## Segurança

**Ipfilter**

Verificando se o serviço do IPFILTER está ativo.
```shell
svcs -a svc:/network/ipfilter:default
```

Habilitando o serviço ipfilter no Solaris10.
```shell
svcadm enable ipfilter
```

Desabilitando o serviço ipfilter no Solaris10.
```shell
svcadm disable ipfilter
```

Diretório e arquivos importantes.
|             |                                                          |
|-------------|----------------------------------------------------------|
| /etc/ipf    | diretório principal                                      |
| ipf.conf    | arquivo que contém as regras                             |
| ippool.conf | arquivo que contém os pools de ip's para usar nas regras |
|             |                                                          |

Ligando o firewall.
```shell
ipf -E
```

Descarrega / recarrega todas as regras temporáriamente, muito útil para facilitar testes.
```shell
ipf -s
```

Desacarregando as regras ( flush ).
```shell
ipf -Fa
```

Desabilitando o firewall.
```shell
ipf -D
```
Sequencia de como ligar e carregar as regras corretamente.
```shell
ipf -E
ippool -F
ippool -f /etc/ipf/ippool.conf
ipf -Fa -f /etc/ipf/ipf.conf
```

Verificar se as regras estão carregadas.
```shell
ipfstat -hio
```

Monitorando a atividade nas regras.
```shell
ipmon
```

**Web console ( desativação )**

Desabilitar o serviço.
```shell
svcs disable svc:/system/webconsole:console
```

Por questões de segurança o serviço pode ser removido após desabilitado.
```shell
svccfg delete svc:/system/webconsole:console
```

Caso se deseje também remover os pacotes, após os passos acima, devem ser removidos com pkgrm todos os pacotes listados pelo comando.
```shell
pkginfo | grep "Web Console"
```

## Imagem / Backup

**Solaris Flash Archive.**

Criando um imagem (.flar ) do sistema.
```shell
flarcreate -c -n nome_da_flar /opt/nome_da_flar.flar
```
|                        |                                    |
|------------------------|------------------------------------|
| flarcreate             | comando                            |
| -c                     | parâmetro para ativar a compressão |
| -n                     | parâmetro para selecionar o nome   |
| nome_da_flar           | nome do .flar                      |
| /opt/nome_da_flar.flar | local onde será criado o .flar     |
|                        |                                    |

## Pacotes / Repositórios

**OpenCSW**

Instalando pkgutil, um repositório público da comunidade para Solaris10.
```shell
mkdir -p /dados/install-src
cd /dados/install-src
/usr/sfw/bin/wget http://mirror.opencsw.org/opencsw/pkgutil.pkg
pkgadd -d pkgutil.pkg
ln -s /opt/csw/bin /usr/bin
pkgutil --catalog
pkgutil --syscheck
```

Examplo de como utilizar o pkgutil.
```shell
pkgutil -i -y vim wget nmap top
```

## Network

Captura e inspeção de pacotes.
```shell
snoop -r
```

Captura e inspeção de pacotes para uma porta especifica.
```shell
snoop -r port 80
```

Comando para realizar uma analise de trafego e salvar em arquivo, para tratar no WIRESHARK.
```shell
snoop -r -o arp11.snoop -q -d nxge0 -c 150000
```

|                |                                          |
|----------------|------------------------------------------|
| snoop          | comando                                  |
| -r             | não resolve DNS do IP para nome          |
| -o arp11.snoop | joga a saída para um arquivo arp11.snoop |
| -q             | não mostra a saída na tela               |
| -d nxge0       | interface de rede                        |
| -c 150000      | captura 15.000 pacotes                   |
|                |                                          |

Ferramentas de IPMI ( Intelligent Platform Management Interface (IPMI) é uma interface padronizada para gerência de hardware ).
```shell
ipmitool
```

Exemplo de como listar os dados da interface de rede.
```shell
ipmitool lan print	
```

## Sistema operacional

Mostra o diagnostico do sistema, também é muito útil para coletar informações do hardware.
```shell
prtdiag
```

Mostra as configurações do sistema.
```shell
prtconf
```

Como colocar um comando em loop de execução.
```shell
yes "clear; ps -ef; sleep 5" | sh
```

Verificando processos rodando.
```shell
pfiles `ptree | awk '{print $1}'` | egrep '^[0-9]|port:'
```

## Memória

Mostra as estatisticas de memória virtual, a cada 1 segundo.
```shell
vmstat 1
```

Mostra as estatísticas de memória virtual.
```shell
echo ::memstat | mdb -k
```

Mostra informação sobre os tipos de virtualizações suportadas pelo hardware.
```shell
virtinfo

    NAME            CLASS
    logical-domain  current
    non-global-zone supported
    kernel-zone     supported
    logical-domain  supported
```

Mostra informação sobre os tipos de virtualizações do sistema.
```shell
virtinfo -a

    Domain role: LDoms control I/O root
    Domain name: primary
    Domain UUID: 974d0c7d-2ab1-4302-ab9c-bae7544bafe0
    Control domain: server01
    Chassis serial#: 0000001
```

Lista os eventuais problemas detectados no servidor.
```shell
fmadm faulty
```
## Processos

Mostra informações de processos por usuáros em um bloco separado.
```shell
prstat -a
```

Mostra os micro estatos de um processo.
```shell
prstat -mL
```

Mostra informações das zones em um bloco separado.
```shell
prstat -Z
```

Mostras as estatísticas de disk I/O.
```shell
iostat
```

Mostras as estatísticas de disk I/O, atualiza os dados a cada 1 segundo e executa o comando 10 vezes depois para.
```shell
iostat -xncpz 1 10
```

Mostras as estatísticas de disk I/O para volumes ZFS.
```shell
zpool iostat -v 1 1000
```

Lista o RAID da máquina, caso ele tenha sido montado atraves do sistema operacional.
```shell
raidctl -l
```

Adicionando data e hora no history de comandos
```console
export HISTTIMEFORMAT="%h/%d – %H:%M:%S"
```



# How to shutdown Solaris 10

---

Command to shutdown immediately your system.
```console
shutdown -y -i5 -g0
```
