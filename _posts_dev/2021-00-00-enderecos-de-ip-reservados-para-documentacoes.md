---
title: "Endereços de IP reservados para documentações"
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

# Reserved IP addresses for use in documentations

**Link to the Wikipedia:**
- <a href="https://en.wikipedia.org/wiki/Reserved_IP_addresses" target="_blank">`https://en.wikipedia.org/wiki/Reserved_IP_addresses`</a> 

---
**IPv4**
**Link to the IANA:**
- <a href="https://www.iana.org/assignments/iana-ipv4-special-registry/iana-ipv4-special-registry.xhtml" target="_blank">`https://www.iana.org/assignments/iana-ipv4-special-registry/iana-ipv4-special-registry.xhtml`</a> 

| Block           | Range                         | Number            | Scope        | Description                                                                                     |
|-----------------|-------------------------------|-------------------|--------------|-------------------------------------------------------------------------------------------------|
| 192.0.2.0/24    | 192.0.2.0 – 192.0.2.255       | 256               | Docmentation | Assigned as "TEST-NET-1" for use in documentation and examples. It should not be used publicly. |
| 198.51.100.0/24 | 198.51.100.0 – 198.51.100.255 | 256               | Docmentation | Assigned as "TEST-NET-2" for use in documentation and examples. It should not be used publicly. |
| 203.0.113.0/24  | 203.0.113.0 – 203.0.113.255   | 256               | Docmentation | Assigned as "TEST-NET-3" for use in documentation and examples. It should not be used publicly. |

---
**IPv6**
**Link to the IANA:**
- <a href="https://www.iana.org/assignments/iana-ipv6-special-registry/iana-ipv6-special-registry.xhtml" target="_blank">`https://www.iana.org/assignments/iana-ipv6-special-registry/iana-ipv6-special-registry.xhtml`</a> 

| Block           | Range                                               | Number            | Scope         | Description                                             |
|-----------------|-----------------------------------------------------|-------------------|---------------|---------------------------------------------------------|
| 2001:db8::/32   | 2001:db8:: – 2001:db8:ffff:ffff:ffff:ffff:ffff:ffff | 2^96              | Documentation | Addresses used in documentation and example source code |
