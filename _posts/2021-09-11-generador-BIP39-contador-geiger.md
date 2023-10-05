---
layout: post
title:  "Generando mnemónicos BIP39 de Bitcoin con un contador Geiger"
date:   2021-09-11 20:00:00 +0200
image:  "/assets/images/GeigerBIP39Generator.png"
category: Bitcoin
permalink: /Bitcoin/:day-:month-:year/:title:output_ext
comments: true
---

Utilizando mi contador Geiger como un [generador de números aleatorios reales](https://en.wikipedia.org/wiki/Hardware_random_number_generator) (valiéndome de la aleatoriedad en el intervalo de tiempo entre las detecciones de dos partículas ionizantes) he programado un pequeño script en Python 3 que utiliza esta aleatoriedad para generar las palabras de un [mnemónico BIP39 de Bitcoin](https://github.com/bitcoinbook/bitcoinbook/blob/develop/ch05.asciidoc#deterministic-seeded-wallets). Esto te permite asegurar tus monedas mediante la aleatoriedad en las desintegraciones de elementos radiactivos en tu entorno y el tiempo de llegada de rayos cósmicos a la atmósfera.

<center> <a href="https://www.youtube.com/watch?v=Qx44_psG9KI"><img src="/assets/images/GeigerBIP39Generator.png" width="600" /></a></center>

<br>
Si quieres echarle un vistazo, puedes encontrarlo en [mi GitHub](https://github.com/danieldemercado/GeigerBIP39Generator).

