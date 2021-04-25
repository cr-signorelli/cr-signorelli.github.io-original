# Identify the Windows version inside a stick or DVD

**Open CMD or PowerShell as administrator** 
```PowerShell
dism /Get-WimInfo /WimFile:D:\sources\install.esd /index:1
```

**Example of the expected output** 
```PowerShell
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
