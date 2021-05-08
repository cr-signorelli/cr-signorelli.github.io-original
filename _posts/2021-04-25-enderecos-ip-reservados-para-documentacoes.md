---
title: "Endereços IP reservados para documentações"
classes: wide
permalink: /enderecos-ip-reservados-para-documentacoes/
header:
  overlay_image: https://source.unsplash.com/ISG-rUel0Uw/1920x300
  og_image: https://source.unsplash.com/ISG-rUel0Uw/120x120
  teaser: https://source.unsplash.com/ISG-rUel0Uw/200x100
  image_description: "Painel com alguns cabos de rede fibras conectados."
  caption: "Imagem: [**Thomas Jensen**](https://unsplash.com/photos/ISG-rUel0Uw)"
  #actions:
  #  - label: "Leia mais"
  #    url: "https://cr-signorelli.github.io/"
excerpt: "Cuidado ao compartilhar suas notas na internet para não expor informações sigilosas de suas redes."
categories:
  - network
tags:
  - network
  - documentação
show_date: false
date: 2021-04-25T17:30:00-03:00
last_modified_at: 2021-04-25T17:30:00-03:00
---

Internet Assigned Numbers Authority (IANA) é responsáveis ​​por coordenar alguns dos elementos-chave que mantêm a Internet funcionando sem problemas. Dentre várias atividades que envolvem a para de redes, eles definiram e reservaram alguns blocos de endereço IP's que devem ser usando apenas em documentações, para facilitar e evitar confusões.
{: style="text-align: justify;"}

Embora exista essa recomendação é muito comum ao lermos um tutorial, artigo ou qualquer outro material técnico e não encontrar esses endereços. Poderíamos passar um tempo discutindo os motivos disso e sobre as boas práticas ao escrever uma documentação. Mas um fato importante para pensar é que muitos desses materiais vem da experiencia, fizeram parte do dia a dia de alguém, o histórico salvo do terminal, aquele rascunho salvo em um bloco notas, um procedimento que funcionou que foi enviado para um amigo. O importante mesmo é que no fim alguém decidiu compartilhar essa informação.
{: style="text-align: justify;"}

Blocos IPv4 reservados para documentação:
{: style="text-align: justify;"}

```console
-----------------------------------------------------------------------------------------------------------------
| Bloco           | Range                         | Números | Descrição                                         |
-----------------------------------------------------------------------------------------------------------------
| 192.0.2.0/24    | 192.0.2.0 – 192.0.2.255       | 256     | "TEST-NET-1" para uso em exemplos e documentação. |
| 198.51.100.0/24 | 198.51.100.0 – 198.51.100.255 | 256     | "TEST-NET-2" para uso em exemplos e documentação. |
| 203.0.113.0/24  | 203.0.113.0 – 203.0.113.255   | 256     | "TEST-NET-3" para uso em exemplos e documentação. |
-----------------------------------------------------------------------------------------------------------------
```

Blocos IPv6 reservados para documentação:
{: style="text-align: justify;"}

```console
-----------------------------------------------------------------------------------------------------------------------------------
| Bloco         | Range                                               | Números | Descrição                                       |
-----------------------------------------------------------------------------------------------------------------------------------
| 2001:db8::/32 | 2001:db8:: – 2001:db8:ffff:ffff:ffff:ffff:ffff:ffff | 2^96    | "TEST-NET" para uso em exemplos e documentação. |
-----------------------------------------------------------------------------------------------------------------------------------
```

#### Referências

> [Wikipedia - Inglês](https://en.wikipedia.org/wiki/Reserved_IP_addresses){:target="_blank"}  
> [Wikipedia - Português](https://pt.wikipedia.org/wiki/Endere%C3%A7o_IP){:target="_blank"}  
> [Link para IANA IPv4](https://www.iana.org/assignments/iana-ipv4-special-registry/iana-ipv4-special-registry.xhtml){:target="_blank"}  
> [Link para IANA IPv6](https://www.iana.org/assignments/iana-ipv6-special-registry/iana-ipv6-special-registry.xhtml){:target="_blank"}  
