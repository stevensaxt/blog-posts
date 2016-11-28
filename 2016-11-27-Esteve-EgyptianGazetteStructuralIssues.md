---
layout: page
subheadline: "Justin Esteve"
title: "Egyptian Gazette Structural Issues"
teaser: "Encoding Limitations"
date: 2016-11-27 <!-- date of post submission -->
categories:
  - technical
author: JustinEsteve <!-- all one word -->
tags:
  - structure
  - encoding
header: no
image: <--! for image-name.png, substitute name you've given your image file -->
  title: blog-images/1905-12-11-p7
  thumb: blog-images/1905-12-11-p7
  homepage: blog-images/1905-12-11-p7
  caption: 1905-12-11-p7 <!-- info about the image, such as date of issue -->
  caption_url: https://github.com/jesteve3/blog-posts/blob/master/1905-12-11-p7.jpg <!-- link-to-page-containing-text? -->>
---
During my week of the Egyptian Gazette, I have several page sevens that are unlike the other pages. These pages focus on travel and mostly have hotel advertisements. However, they also contain commentary about the best way to travel to different places and the differences in climate between the two. This commentary is split between all six columns of the newspaper. There are some ![advertisements](1905-12-11-p8-Structure.png) that are similar in that they take up two columns. However, I have no idea how to encode these page sevens in order to reflect the structure of the newspaper. Up till now, I have just grouped the commentary in one div. If I encoded the page sevens by columns, I would get six random groups of text. Obviously, this would cause a great deal of confusion unless there was a way to connect the text in each column together. A tag that connects the text together might work, but this issue is definitely one worth exploring as we move forward.
