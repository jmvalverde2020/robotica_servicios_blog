---
layout: post
title:  "Amazon Warehouse"
summary: Programar un robot de amazon que navegue hasta una estantería, la coga y la lleve al inicio
author: juanmi
date: '2024-06-07 20:44:30 +0200'
category: amazon, OMPL, global navigation, obstacle avoidance, geometry
thumbnail: /assets/img/posts/amazon-warehouse/amazon_warehouse.png
keywords: ompl, amazon, warehouse, navigation
usemathjax: true
permalink: /blog/amazon-warehouse/
---

## Objetivos :

El objetivo de esta práctica es programar un robot de Amazon para almacenes que navegue hasta una estantería, la levante y la lleve al punto inicial usando la librería OMPL que permite encontrar un camino entre dos puntos teniendo en cuenta la geometría del robot.

## OMPL :

OMPL (Open Motion Planning Library) es una librería de Python usada para planificar rutas que permite al usuario especificar cuando un estado es válido para así tener control de, por ejemplo, la geometría del robot.

Tiene distintas opciones de algoritmos de búsqueda como RRT o PRM entre otros.

### Definición del problema :

Para encontrar una ruta con OMPL, es necesario definir el estado especificando los bordes de la zona donde se va a mover el robot. En este caso, está configurado para un almacén de 20.6m de largo y 13.6m de ancho. El mundo en cuestión es el que se muestra a continuación:

<img src="../../assets/img/posts/amazon-warehouse/warehouse.png" width="800" height="800">

Tras definir el espacio, es necesario definir el estado inicial y el objetivo. Para esto, se toma como estado objetivo las coordenadas del robot antes de la ejecución, y como estado objetivo, las coordenadas de una estantería.

### Validez de un estado :

La principal diferencia de OMPL respecto a otras librerías de planificación, es que permite decidir la validez de un estado mediante una función.
A la hora de configurar el planificador, se debe especificar la función que usará para validar cada estado, la cual deberemos implementar nosotros.
En este caso, ya que existe un cambio de geometría, hay dos funciones para comprobar la validez, dependiendo de si ha cogido o no la estantería. Aunque ambas hacen uso de un mapa del mundo donde aparecen los obstáculos, la forma de tratarlo cambia.

En caso de ir el robot solo, se ensanchan las paredes del mapa y se trata al robot como un punto, por lo que, en cada estado, se comprueba la posición del robot en el mapa y se devuelve "False" en caso de haber un obstáculo en dicha posición. 

Por el contrario, si el robot lleva la estantería, se obtiene la posición del robot en un mapa sin ensanchar y se comprueban todos los píxeles que pertenecen a la estantería. Para lograr esto, se usa las medidas de la estantería, con un poco de margen, para, desde la posición del robot comprobar todos los píxeles en los que se encuentra la estantería.

Dado que la orientación del robot influye, para obtener los píxeles se usa una matriz de rotación.
Por otro lado, dado que la estantería que se levanta pasa a formar parte del robot, se elimina del mapa para que no sea tratada como obstáculo.

Además, al llegar el robot al objetivo, se orienta de forma que la estantería es menos ancha que larga para facilitar el camino de vuelta.

El mapa usado sin tratar es el siguiente:

<img src="../../assets/img/posts/amazon-warehouse/map.png" width="800" height="800">

## Movimiento del robot :

Para mover el robot, dado que el planificador devuelve un recorrido por el que no hay obstáculos, se pasan las coordenadas de cada punto del recorrido a coordenadas relativas respecto al robot, con las cuales se obtiene el ángulo y el módulo. Habiendo obtenido ambas cosas, el robot únicamente debe girar el ángulo obtenido, con un límite de velocidad, y dirigirse con la velocidad especificada por el módulo, manteniendose en un límite para evitar ir demasiado rápido.

## Paso de coordenadas de Gazebo al mapa :

Ya que es necesario encontrar varios puntos del mundo de Gazebo en el mapa, es necesario hallar la expresión que correlaciones ambas coordenadas. Para esto se han elegido varios puntos significativos del mapa y se han obtenido sus coordenadas en el mundo de Gazebo. Con ambas coordenadas de cada punto es posible realizar una regresión lineal que nos muestra la expresión que buscábamos.

Los puntos elegidos son los siguientes:

<img src="../../assets/img/posts/amazon-warehouse/Puntos_referencia.png" width="800" height="800">

## Video de demonstración :


<video src="../../assets/img/posts/amazon-warehouse/video_amazon_warehouse.webm" controls="controls" style="max-width: 800px;">
</video>