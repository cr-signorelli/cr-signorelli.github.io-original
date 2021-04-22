---
title: "Solaris 10 - Como criar pacotes .pkg"
classes: wide
#header:
#  overlay_image: /assets/images/pencil-1891732-optimize.jpg
#  og_image: /assets/images/pencil-1891732-optimize.jpg
#  teaser: /assets/images/pencil-1891732_optimize-thumb.jpg
#  image_description: ""
#  caption: "Photo credit: [**congerdesign**](https://pixabay.com/en/pencil-notes-chewed-paper-ball-1891732/)"
#  #actions:
#  #  - label: "Leia mais"
#  #    url: "https://cr-signorelli.github.io/about/por-onde-comecar/"
categories:
  - solaris-10
tags:
  - solaris-10
  - sparc
  - sysadmin
last_modified_at: 2021-04-21T21:30:00-03:00
---

Criar pacotes para instalação no Oracle Solaris-10 Sparc/x86 pode ser muito útil, principalmente porque não existe um sistema de repositório oficial para instalação de pacotes.
{: style="text-align: justify;"}

Normalmente em ambientes de produção o Solaris-10 é instalado apenas com o pacotes minímos necessários, por questões de performance, segurança e economia de espaço em disco. Com isso quando for preciso adicionar algum pacote que foi removido durante ou depois da instalação no Sistema será preciso utilizar o CD/DVD ou uma imagem ISO.
{: style="text-align: justify;"}

```console
+---------------------------------------------------------------------------+
|Select the Solaris software to install on the system                       |
|                                                                           |
|Note: After selecting a software group, you can add or remove              |
|software by customizing it. However this requires understanding of         |
|software dependencies and how Solaris software is packaged.                |
|                                                                           |
|  [ ] Entire Distribution plus OEM support ......8575.00 MB                |
|  [X] Entire Distribution........................8529.00 MB                |
|  [ ] Developer System Support...................8336.00 MB                |
|  [ ] End User System Support....................7074.00 MB                |
|  [ ] Core System Support........................3093.00 MB                |
|  [ ] Reduced Networking Core System Support.....3035.00 MB                |
|                                                                           |
|    F2_Continue      F6_Help                                               |
+---------------------------------------------------------------------------+
```

O processo para criar os pacotes de instalação .pkg podem ser realizando em qualquer outra máquina com Solaris-10 instalado.
{: style="text-align: justify;"}

Baixo a imagem ISO do Solaris-10 Sparc/x86 no site oficial da Oracle.  
<https://www.oracle.com/solaris/solaris10/downloads/solaris10-get-jsp-downloads.html>

Associe o arquivo **sol-10-u11-ga-sparc-dvd.iso** em um "loopback file driver":  

```console
-bash-3.2#  lofiadm -a /tmp/sol-10-u11-ga-sparc-dvd.iso 
```

---

Monte o dispositivo virtual, e acesse o diretório:

```console
-bash-3.2# mount -F hsfs -o ro /dev/lofi/1 /mnt
-bash-3.2# cd /mnt/Solaris_10/
```

---

Usando o comando **pkgtrans** crie o arquivo .pkg com base no pacote **SUNWsshu**  
_SUNWsshu – Contains client files and utilities for the /usr directory_

```console
-bash-3.2# pkgtrans -s Product /var/tmp/ssh-client.pkg SUNWsshu
```

---

Testando se o arquivo foi criado com sucesso.

```console
-bash-3.2# cd /var/tmp/
-bash-3.2# pkgadd -d ./ssh-client.pkg
```

---

**Dica:** Deixe os arquivos .pkg de cada pacote criado previamente, ou pelo menos os pacotes que julgarem mais importantes.
{: .notice--info}

**Alerta:** Cuidado ao usar esses arquivos para reinstalar algum pacote no Servidor, podem haver diferenças nas versões, principalmente se o Solaris-10 já tenha recebido alguma atualização via **10_Recommended.zip**
{: .notice--danger}

**Referencias:**
Lista oficial da Oracle com os pacotes do Solaris-10 [https://docs.oracle.com/cd/E19253-01/html/817-0545/index.html](https://docs.oracle.com/cd/E19253-01/html/817-0545/index.html)

Biblioteca pública e Arquivo Digital:
* [https://www.ibiblio.org/](https://www.ibiblio.org/)
* [http://www.ibiblio.org/pub/packages/solaris/sparc/](http://www.ibiblio.org/pub/packages/solaris/sparc/)
* [http://www.ibiblio.org/pub/packages/solaris/i86pc/](http://www.ibiblio.org/pub/packages/solaris/i86pc/)
* [https://www.ibiblio.org/pub/packages/solaris/i86pc/html/creating.solaris.packages.html](https://www.ibiblio.org/pub/packages/solaris/i86pc/html/creating.solaris.packages.html)
* [https://www.ibiblio.org/pub/packages/solaris/sparc/html/creating.solaris.packages.html](https://www.ibiblio.org/pub/packages/solaris/sparc/html/creating.solaris.packages.html)