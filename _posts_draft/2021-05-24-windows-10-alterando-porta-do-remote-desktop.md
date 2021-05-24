---
title: "Solaris 10 - Espelhamento de disco no ZFS"
classes: wide
permalink: /windows-10-alterando-porta-do-remote-desktop/
header:
  overlay_filter: 0.5
  overlay_image: https://source.unsplash.com/48yI_ZyzuLo/1920x300
  og_image: https://source.unsplash.com/48yI_ZyzuLo/120x120
  teaser: https://source.unsplash.com/48yI_ZyzuLo/200x100
  image_description: "Foto de um prédio espelhado"
  caption: "Imagem: [**Michael**](https://unsplash.com/photos/48yI_ZyzuLo)"
  #actions:
  #  - label: "Leia mais"
  #    url: "https://cr-signorelli.github.io/"
excerpt: "Acessar remotamente seu sistema pode ser um desafio por causa de bloqueios de segurança internos e externos da sua rede"
categories:
  - windows-10
tags:
  - windows-10
  - remote-desktop
  - network
show_date: false
date: 2021-05-24T20:00:00-03:00
last_modified_at: 2021-05-24T20:00:00-03:00
---

Para alterar a porta para uma conexão específica no Terminal Server:

Execute Regedt32 e vá a esta chave:
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp
    OBSERVAÇÃO: A chave do registro acima é um caminho; ela foi quebrada para fins de legibilidade.

Localize a subchave de "PortNumber" e observe o valor de 00000D3D, hex para (3389). Modifique o número da porta em Hex e salve o novo valor.


Remote desktop protocol (RDP) is the de facto administrative console access, and it may be necessary to make it even more secure by changing the TCP port used for the network access. It's also useful when the remote computer is behind firewall which doesn't allow incoming and outgoing connections other than standard ports or users unable to configure the port forwarding for Remote Desktop if they're behind firewall or router's NAT.

RDP transports on TCP 3389 by default for all supported versions of Windows; if you want to change the port, it requires a quick change in the Windows registry.

Note: Editing the registry is risky, so be sure you have a verified backup before saving any changes.



It may require a reboot to make the port assignment take effect. Once the system is listening on the new port, connections need to specify the new port in the RDP client properties, as shown in following image.

The Windows Server system will now listen on the new port with the Svchost.exe process, visible in task manager by entering Netstat  -a -n -o to view the current processes and list the associated executable.



{: style="text-align: justify;"}

**Alerta:** Mesmo realizando o processo de upgrade via software será preciso reiniciar o servidor.
{: .notice--danger}

