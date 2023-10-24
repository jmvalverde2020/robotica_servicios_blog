---
layout: post
title:  "Localized Vacuum Cleaner"
summary: Programar un robot aspiradora con algoritmos de autolocalización
author: juanmi
date: '2023-10-03 11:30:12 +0200'
category: vacuum cleaner, localized
thumbnail: /assets/img/posts/vacuum_cleaner.png
keywords: vacuum cleaner, localized, cleaner, aspiradora, localizada, robot aspiradora
usemathjax: true
permalink: /blog/vacuum-cleaner/
---

## Objetivos :

El objetivo de esta práctica es programar un robot aspiradora capaz de autolocalizarse para hacer más eficiente su algoritmo y limpiar así la casa
en menos tiempo usando el algoritmo BSA de cobertura

## BSA :

El algoritmo BSA _(Backtracking Spiral Algorithm)_ es un algoritmo de cobertura para robots móviles basado en el uso de una espiral (o barrido en este caso), que completa toda una zona antes de pasar a la siguiente, asegurando así la cobertura de todo el escenario.

Hay una serie de puntos a tener en especial consideración:
- Puntos críticos: Aquellos puntos cuyas celdas contiguas han sido todas visitadas,
- Puntos de retorno: Son los puntos no visitados más cercanos al punto crítico.
- Obstáculos: Puntos por los que no puede pasar el robot
- Casillas libres: Las casillas por las que pasa el robot

Con este algoritmo, el robot recorrerá la zona pasando a una casilla adyacente cada vez y al encontrarse en un punto crítico, encontrará el camino más corto al punto de retorno más cercano.

Un ejemplo la ejecución de este algoritmo es el siguiente:

<div style="position: relative; padding-bottom: 56.25%; height: 0;"><iframe src="https://jumpshare.com/embed/8ntvBXXYcdfrKgIe9kpd" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe></div>

