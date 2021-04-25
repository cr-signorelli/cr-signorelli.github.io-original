# Windows - alterando a porta do Remote Desktop

---

Acesse o registro do Windows ( regedit ), e localize a chave abaixo:
```console
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp
```

Localize a subchave de "PortNumber" e observe o valor de 00000D3D, hex para (3389). Modifique o número da porta em Hex e salve o novo valor.
