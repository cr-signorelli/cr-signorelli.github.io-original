---
title: "Solaris 10 - Ajustando TIMEZONE"
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
  - network
tags:
  - network
  - documentação
last_modified_at: 2021-04-22T23:00:00-03:00
---

# Ajustando TIMEZONE

---

Criar um arquivo com este conteúdo (de preferência /usr/share/zoneinfo/Brazil.rules).
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

----

Compile com o comando zic, isto irá gerar o arquivo East dentro do diretório Brazil.
```console
-bash-3.2# zic /usr/share/zoneinfo/Brazil.rules
```

Para testar.
```console
-bash-3.2# zdump -v Brazil/East | grep 201[01]
```

Se tudo der certo irá exibir algo parecido com isso.
```console
Brazil/East  Sun Oct 17 02:59:59 2010 UTC = Sat Oct 16 23:59:59 2010 BRT  isdst=0 gmtoff=-10800
Brazil/East  Sun Oct 17 03:00:00 2010 UTC = Sun Oct 17 01:00:00 2010 BRST isdst=1 gmtoff=-7200
Brazil/East  Sun Feb 20 01:59:59 2011 UTC = Sat Feb 19 23:59:59 2011 BRST isdst=1 gmtoff=-7200
Brazil/East  Sun Feb 20 02:00:00 2011 UTC = Sat Feb 19 23:00:00 2011 BRT  isdst=0 gmtoff=-10800
Brazil/East  Sun Oct 16 02:59:59 2011 UTC = Sat Oct 15 23:59:59 2011 BRT  isdst=0 gmtoff=-10800
Brazil/East  Sun Oct 16 03:00:00 2011 UTC = Sun Oct 16 01:00:00 2011 BRST isdst=1 gmtoff=-7200
```

Substituir o arquivo localtime da maquina pelo arquivo gerado.
```console
-bash-3.2# cp /usr/share/zoneinfo/Brazil/East /etc/localtime
```

Para saber qual a TIMEZONE está sendo utilizada.
```console
-bash-3.2#  grep '^TZ' /etc/TIMEZONE
```

TIMEZONEs estão disponiveis.
```console
-bash-3.2#  ls /usr/share/lib/zoneinfo
```

Alterandoo TIMEZONE.
```
Altere a linha TZ= /etc/TIMEZONE e reinicie o sistema.
```

---

**Referências:**

- <a href="http://www.timeanddate.com/" target="_blank">`http://www.timeanddate.com/`</a> 
- <a href="http://sysunconfig.net/unixtips/timezone.txt" target="_blank">`http://sysunconfig.net/unixtips/timezone.txt`</a> 

