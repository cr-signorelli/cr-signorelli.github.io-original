---
title: "Solaris 10 - Ajustando TIMEZONE"
classes: wide
permalink: /solaris-10-ajustando-timezone/
header:
  overlay_image: https://source.unsplash.com/CcIIao_-Eow/1920x300
  og_image: https://source.unsplash.com/CcIIao_-Eow/120x120
  teaser: https://source.unsplash.com/CcIIao_-Eow/200x100
  image_description: "Foto preta e branca de um relógio de corda."
  caption: "Imagem: [**Amanda Jones**](https://unsplash.com/photos/CcIIao_-Eow)"
  #actions:
  #  - label: "Leia mais"
  #    url: "https://cr-signorelli.github.io/"
excerpt: "Manter o horário de um servidor correto é crucial para o funcionamento de diversos sistemas e para muitos negócios."
categories:
  - solaris-10
tags:
  - solaris-10
  - sparc
  - sysadmin
show_date: false
date: 2021-04-25T00:30:00-03:00
last_modified_at: 2021-04-25T00:30:00-03:00
---

Caso você não esteja familiarizado com administração de servidores pode não notar a importância dessa tarefa. Imagine logs e transações registrados com tempo errado, ao finalizar o horário de verão logs informações diferentes registradas com a mesma data e hora. Seria possível elencar diversas situações críticas, mas para ilustrar melhor pense em um sistema bancário e quanto ele pode ser afetado.
{: style="text-align: justify;"}

Crie um arquivo com seguite conteúdo (de preferência **/usr/share/zoneinfo/Brazil.rules**):
{: style="text-align: justify;"}

```console
#Rule   NAME    FROM    TO      TYPE    IN      ON      AT      SAVE    LETTER/S
Rule    Brazil  2010    max     -       Oct     Sun>=15 00:00   1       S
Rule    Brazil  2011    only    -       Feb     Sun>=15 00:00   0       -
Rule    Brazil  2012    only    -       Feb     26      00:00   0       -       #Carnaval 3° Dom
Rule    Brazil  2013    2014    -       Feb     Sun>=15 00:00   0       -
Rule    Brazil  2015    only    -       Feb     22      00:00   0       -       #Carnaval 3° Dom
Rule    Brazil  2016    2022    -       Feb     Sun>=15 00:00   0       -
Rule    Brazil  2023    only    -       Feb     26      00:00   0       -       #Carnaval 3° Dom
Rule    Brazil  2024    2025    -       Feb     Sun>=15 00:00   0       -
Rule    Brazil  2026    only    -       Feb     22      00:00   0       -       #Carnaval 3° Dom
Rule    Brazil  2027    2033    -       Feb     Sun>=15 00:00   0       -
Rule    Brazil  2034    only    -       Feb     26      00:00   0       -       #Carnaval 3° Dom
Rule    Brazil  2035    2036    -       Feb     Sun>=15 00:00   0       -
Rule    Brazil  2037    only    -       Feb     22      00:00   0       -       #Carnaval 3° Dom
Rule    Brazil  2038    only    -       Feb     Sun>=15 00:00   0       -
Rule    Brazil  2039    only    -       Feb     27      00:00   0       -       #Carnaval 3° Dom
Rule    Brazil  2040    2044    -       Feb     Sun>=15 00:00   0       -
Rule    Brazil  2045    only    -       Feb     26      00:00   0       -       #Carnaval 3° Dom
Rule    Brazil  2046    2047    -       Feb     Sun>=15 00:00   0       -
Rule    Brazil  2048    only    -       Feb     23      00:00   0       -       #Carnaval 3° Dom
Rule    Brazil  2049    only    -       Feb     Sun>=15 00:00   0       -
Rule    Brazil  2050    only    -       Feb     27      00:00   0       -       #Carnaval 3° Dom

# Zone  NAME            GMTOFF  RULES/SAVE      FORMAT [UNTIL]
Zone    Brazil/East     -3:00   Brazil          BR%sT
````

Compile com o comando **zic**, isto irá gerar o arquivo East dentro do diretório Brazil:
{: style="text-align: justify;"}

```console
-bash-3.2# zic /usr/share/zoneinfo/Brazil.rules
```

Utilizando o comando **zdump** teste o arquivo, se tudo estiver correto um resultado similar aparecerá:
{: style="text-align: justify;"}

```console
-bash-3.2# zdump -v Brazil/East | grep 201[01]

Brazil/East  Sun Oct 17 02:59:59 2010 UTC = Sat Oct 16 23:59:59 2010 BRT  isdst=0 gmtoff=-10800
Brazil/East  Sun Oct 17 03:00:00 2010 UTC = Sun Oct 17 01:00:00 2010 BRST isdst=1 gmtoff=-7200
Brazil/East  Sun Feb 20 01:59:59 2011 UTC = Sat Feb 19 23:59:59 2011 BRST isdst=1 gmtoff=-7200
Brazil/East  Sun Feb 20 02:00:00 2011 UTC = Sat Feb 19 23:00:00 2011 BRT  isdst=0 gmtoff=-10800
Brazil/East  Sun Oct 16 02:59:59 2011 UTC = Sat Oct 15 23:59:59 2011 BRT  isdst=0 gmtoff=-10800
Brazil/East  Sun Oct 16 03:00:00 2011 UTC = Sun Oct 16 01:00:00 2011 BRST isdst=1 gmtoff=-7200
```

Substitua o arquivo **localtime** da maquina pelo novo, mas sempre faça um backup do original por segurança:
{: style="text-align: justify;"}

```console
-bash-3.2# cp /etc/localtime /etc/localtime-backup
-bash-3.2# cp /usr/share/zoneinfo/Brazil/East /etc/localtime
```

Verifique qual o TIMEZONE está sendo utilizado:
{: style="text-align: justify;"}

```console
-bash-3.2#  grep '^TZ' /etc/TIMEZONE
```

Listando todos os TIMEZONE que estão disponiveis:
{: style="text-align: justify;"}

```console
-bash-3.2#  ls /usr/share/lib/zoneinfo
```

Por último edite o arquivo **/etc/TIMEZONE** e altere o conteúdo da linha **TZ=** para o TIMEZONE escolhido. Se possível reiniciei o sistema para confirmar se tudo está funcionando corretamente.
{: style="text-align: justify;"}

**Dica:** Caso seu sistema operacional esteja sincronizado com um servidor NTP e mesmo ajustando o TIMEZONE o horário não sincronizar novamente ajuste manualmente, se a diferença (drift) for grande não voltar automaticamente.
{: .notice--info}

#### Referências

> [http://www.timeanddate.com/](http://www.timeanddate.com/){:target="_blank"}  
> [http://sysunconfig.net/unixtips/timezone.txt](http://sysunconfig.net/unixtips/timezone.txt){:target="_blank"}  
