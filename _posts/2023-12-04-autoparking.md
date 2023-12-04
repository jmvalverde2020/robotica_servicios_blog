---
layout: post
title:  "Autoparking"
summary: Programar un algoritmo que permita aparcar un coche de forma autónoma
author: juanmi
date: '2023-12-04 17:56:30 +0200'
category: autoparking, autonomous car
thumbnail: /assets/img/posts/autoparking/autoparking.png
keywords: autoparking, car, autonomous, coche, aparcar
usemathjax: true
permalink: /blog/autoparking/
---

## Objetivos :

El objetivo de esta práctica es programar un algoritmo que permita aparcar un coche de forma autónoma en una variedad de situaciones. Por ejemplo, habiendo el hueco justo, sin tener un coche delante como referencia, o sin haber coches en la calle.

## Fases del aparcamiento :

A la hora de aparcar un coche, es necesario antes haberse aproximado, haber encontrado el aparcamiento, y haber colocado el coche para iniciar la maniobra. Esto nos deja con 4 fases principales necesarias:

- Aproximarse a la línea de aparcamiento, o a los coches aparcados y seguir la dirección.
- Detectar cuando hay un aparcamiento en el que quepamos.
- Adelantarse para colocar el coche en una buena posición.
- Iniciar la maniobra

### Aproximación :

Esta puede que sea la fase más compleja ya que con los láseres proporcionados no podemos detectar la línea de aparcamiento y por lo tanto es necesario para orientarnos que haya algún tipo de referencia, como por ejemplo, los mismos coches aparcados.

Habiendo referencias, se puede calcular la dirección de la calle en base a las medidas del láser usando técnicas como la regresión lineal. 

En este caso se trata únicamente de un controlador proporcional que mantiene el coche a una distancia de 1.5m del obstáculo que detecte a su derecha. 
Una vez se ha estabilizado, nos guardamos la dirección.

Esto ya es suficiente para encontrar la dirección de la calle y seguirla en caso de que deje de haber coches de referencia.

### Detección :

Es importante detectar los huecos suficientemente grandes como para que entre el coche.

Para esta tarea hacemos uso del láser, comprobando que no se detecten obstáculos a la derecha del coche con ninguno de los tres láseres disponibles, esto nos dice que el hueco es lo sufientemente grande para que quepa nuestro vehículo.

<img src="../../assets/img/posts/autoparking/Deteccion.png" width="800" height="800">

### Preparación :

Para preparar el coche para la maniobra de aparcar, es necesario adelantarlo más o menos el tamaño del propio coche o un poco más, teniendo en cuenta que los coches son de tamaño similar, se comprueba cuando el coche está un poco adelantado al de enfrente, usando los láseres.

En caso de que el coche de delante sea demasiado largo, o no haya, se tomará como referencia un tiempo ideal calculado previamente. Este tiempo es suficiente para aparcar correctamente.

<img src="../../assets/img/posts/autoparking/Preparacion.png" width="800" height="800">

### Aparcamiento :

Hay muchas formas y métodos para aparcar un coche. La elegida aquí consta de 3 pasos:

- Giro
- Retroceso
- Alineación

#### Giro
El giro se refiere a girar el coche marcha atrás hasta que el culo del coche quede libre, es decir, que podamos circular marcha atrás sin chocar con el coche que tenemos detrás.

<img src="../../assets/img/posts/autoparking/Giro.png" width="800" height="800">


#### Retroceso
El siguiente paso es retroceder hasta que el morro de nuestro coche pueda entrar sin chocar con el coche de delante, para esto usamos las medidas del laser delantero como referencia.


#### Alineación
Para alinear el coche giramos, aún retrocediendo, para meter el morro en el aparcamiento.

En el caso en el que nos acercemos demasiado al coche de detrás, giramos en sentido y marcha contrarios para seguir alineándonos, esta vez hacia delante.

\
Todas estas fases han sido medidas en tiempo con anterioridad para calcular el tiempo ideal de cada una y poder usarlo como referencia en caso de no poder usar el láser.


<img src="../../assets/img/posts/localized-vacuum-cleaner/Retroceso.png" width="800" height="800">


## Principales Problemas :


### Regresión lineal y PID


Este método de aproximación, si bien es el más exacto, es mucho más difícil de implementar que el usado aquí, por lo que al final no se ha llevado a cabo debido a su complejidad.

### Tiempos


El cálculo de tiempos ha tenido que ser medido y redondeando tras varias pruebas ya que pueden influir varios factores que cambian el resultado de la maniobra, los elegidos han sido los que mejor resultado han obtenido.


## Videos de demonstración :

### Con coches a ambos lados


<video src="../../assets/img/posts/autoparking/1-Hueco.webm" controls="controls" style="max-width: 800px;">
</video>

### Con solo un coche detrás


<video src="../../assets/img/posts/autoparking/Sin-coche-delante.webm" controls="controls" style="max-width: 800px;">
</video>

### Sin coches en la calle


<video src="../../assets/img/posts/autoparking/Sin-coches.webm" controls="controls" style="max-width: 800px;">
</video>