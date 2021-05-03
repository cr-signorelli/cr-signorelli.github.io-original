---
title: "Windows 10 - Identificado a versão no DVD ou pendrive"
classes: wide
header:
  overlay_image: /assets/images/windows10-optimize.jpg
  og_image: /assets/images/windows10-optimize-og.jpg
  teaser: /assets/images/windows10-optimize-thumb.jpg
  image_description: "Fundo azul com o logo da microsoft a direita"
  #caption: "Foto/Imagem: [**signorelli**](https://pixabay.com/)"
#  #actions:
#  #  - label: "Leia mais"
#  #    url: "https://cr-signorelli.github.io/"
categories:
  - solaris-11.3
tags:
  - solaris-11.3
  - sparc
  - sysadmin
last_modified_at: 2021-05-02T20:00:00-03:00
---

Antes de instalar o Windows em um computador é importante verificar qual a versão que está usando antes, isso pode te poupar um tempo valioso com Updates de sistema.
{: style="text-align: justify;"}

Abra o **powershell** como administrador e execute o comando abaixo, lembre-se que **D:** é a unidade onde está seu DVD ou pendrive que será analisado:
{: style="text-align: justify;"}

```PowerShell
dism /Get-WimInfo /WimFile:D:\sources\install.esd /index:1

Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Windows\system32> dism /Get-WimInfo /WimFile:D:\sources\install.esd /index:1

Deployment Image Servicing and Management tool
Version: 10.0.17763.1

Details for image : D:\sources\install.esd

Index : 1
Name : Windows 10 Home
Description : Windows 10 Home
Size : 13,998,570,900 bytes
WIM Bootable : No
Architecture : x64
Hal : <undefined>
Version : 10.0.18362
ServicePack Build : 30
ServicePack Level : 0
Edition : Core
Installation : Client
ProductType : WinNT
ProductSuite : Terminal Server
System Root : WINDOWS
Directories : 18679
Files : 87458
Created : 4/1/2019 - 8:09:02 PM
Modified : 5/21/2019 - 5:19:07 PM
Languages : en-US (Default)

The operation completed successfully.
PS C:\Windows\system32>
```
