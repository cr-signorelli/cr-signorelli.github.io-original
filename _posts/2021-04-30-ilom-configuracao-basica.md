---
title: "ILOM - Configuração básica"
classes: wide
header:
  overlay_image: /assets/images/network-3849206-optimize.jpg
  og_image: /assets/images/network-3849206-optimize-og.jpg
  teaser: /assets/images/network-3849206-optimize-thumb.jpg
  image_description: "Fundo branco com linhas conectadas formando uma rede"
  #caption: "Foto/Imagem: [**signorelli**](https://pixabay.com/)"
  #actions:
  #  - label: "Leia mais"
  #    url: "https://cr-signorelli.github.io/"
categories:
  - ilom
tags:
  - ilom
  - network
  - sparc
  - sysadmin
last_modified_at: 2021-04-29T18:00:00-03:00
---

#### Informações e Requisitos

> O procedimento descrito foi realizado em um Servidor Oracle T-Series SPARC.  
> Servidor dever estar utilizando ILOM como firmware de gerenciamento.  
> Acesso de gerenciamento remoto (NET-MGT) ou via console (SER-MGT).  

#### Configurando IP address na NET-MGT

Uma vez logado na console da ILOM use os comandos abaixo para configurar a rede.

```console
> cd /SP/network
> set pendingipaddress=192.0.2.10
> set pendingipnetmask=255.255.255.0 
> set pendingipgateway=192.0.2.1
> set commitpending=true
```

#### Troca de senha

Por padrão os servidores Oracle SPARC a ILOM vem com usuário e senha pré definidos:

> usuário: **root**  
> senha: **changeme**  

Uma vez logado na console da ILOM use o comando abaixo para trocar a senha de root:

```console
> set /SP/users/root <senha_nova>
```

#### Update via CLI (command line)

Requisitos para essa etapa:

> Um computador conectado na mesma rede e no mesmo domínio de broadcast do servidor.  
> Um servidor web instalado e habilitado com a porta 8888 liberada.  
> Hospedar no servidor web o arquivo do firmware que será utilizado.  

Através da console inicie o processo:

```console
-> load -source http://192.0.2.10:8888/Sun_System_Firmware-9_8_5_c-SPARC_T7-2.pkg
```

```console
NOTE: An upgrade takes several minutes to complete. ILOM
      will enter a special mode to load new firmware. No
      other tasks can be performed in ILOM until the
      firmware upgrade is complete and ILOM is reset.

Are you sure you want to load the specified file (y/n)? y
Preserve existing configuration (y/n)? y

Firmware update is complete.
ILOM will now be restarted with the new firmware.
```

```console
-> /sbin/reboot
```

#### Clean registers

Reinicia tudo o sistema da ILOM, é muito similar a um **"power cycle"**.

Acesse o "prompt ok" (OBP = OpenbootProm) use o comando **.registers** para mostrar os dados dos registradores:

```console
{0} ok .registers
```

Limpando registradores:

```console
{0} ok reset-all
```

#### Referências

> [Oracle Integrated Lights Out Manager (ILOM) 3.0 CLI Procedures Guide](https://docs.oracle.com/cd/E19201-01/820-6412-12/backuprestore_cli.html)  
> [Procedure to reset SP](https://docs.oracle.com/cd/E19201-01/820-6412-12/backuprestore_cli.html#50561097_Restore%20the%20ILOM%20Configuration)  
> [Post extended configuration for T7-1](https://docs.oracle.com/cd/E54976_01/html/E54980/z4000ca11317576.html#scrolltoc)  
