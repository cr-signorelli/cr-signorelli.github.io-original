---
title: "Bem vindo ao Beco do Código"
layout: splash
permalink: /splash-page/
date: 2021-05-07T01:00:00-03:00
last_modified_at: 2021-05-07T01:00:00-03:00
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: https://source.unsplash.com/WLUHO9A_xik
  actions:
    - label: "Leia mais"
      url: "/terms/"
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
excerpt: "Está página ainda está em desenvolvimento"
intro: 
  - excerpt: 'Está página ainda está em desenvolvimento. Centered with `type="center"`'
feature_row:
  - image_path: https://source.unsplash.com/CBFXuVLSe8U
    image_caption: "Image courtesy of [Unsplash](https://unsplash.com/)"
    alt: "placeholder image 1"
    title: "Placeholder 1"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
  - image_path: https://source.unsplash.com/PaEiKmY697Y
    alt: "placeholder image 2"
    title: "Placeholder 2"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"
  - image_path: https://source.unsplash.com/KRJ5EurQnRA
    title: "Placeholder 3"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
feature_row2:
  - image_path: https://source.unsplash.com/I4ScSrKsfIg
    alt: "placeholder image 2"
    title: "Placeholder Image Left Aligned"
    excerpt: 'This is some sample content that goes here with **Markdown** formatting. Left aligned with `type="left"`'
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"
feature_row3:
  - image_path: https://source.unsplash.com/cH2iaFLi9vw
    alt: "placeholder image 2"
    title: "Placeholder Image Right Aligned"
    excerpt: 'This is some sample content that goes here with **Markdown** formatting. Right aligned with `type="right"`'
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"
feature_row4:
  - image_path: https://source.unsplash.com/poED7Zsm5n4
    alt: "placeholder image 2"
    title: "Placeholder Image Center Aligned"
    excerpt: 'This is some sample content that goes here with **Markdown** formatting. Centered with `type="center"`'
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"
---

{% include feature_row id="intro" type="center" %}

{% include feature_row %}

{% include feature_row id="feature_row2" type="left" %}

{% include feature_row id="feature_row3" type="right" %}

{% include feature_row id="feature_row4" type="center" %}