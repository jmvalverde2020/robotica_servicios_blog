---
layout: post
title:  "Rescue Dron"
summary: Programar un dron de rescate capaz de reconocer caras y enviar su posición
author: juanmi
date: '2024-01-16 17:21:30 +0200'
category: dron, rescue, face recognition, face detection
thumbnail: /assets/img/posts/rescue-dron/rescue-dron.png
keywords: rescue-dron, dron, face detection, rescate
usemathjax: true
permalink: /blog/rescue-dron/
---

## Objetivos :

El objetivo de esta práctica es programar un dron de rescate. Al principio, será dada la localización aproximada de los náufragos, por lo que el dron deberá dirigirse a esas coordenadas y barrer la zona en busca de las 6 personas, detectando las caras y enviando la posición al encontrar a cada una.

## Detección de caras :

Para detectar las caras, usamos la librería de OpenCV, haarcascade. Su uso es bastante simple, puesto que es suficiente con iniciar y entrenar el clasificador, en este caso usando el fichero de entrenamiento "haarcascade_frontalface_default.xml", y pasar la imagen en la que se quieran detectar las caras así como un par de argumentos que es conveniente cambiar.

Estos argumentos son:
- minSize: Se trata del tamaño mínimo de la cara a detectar, en caso de ser demasiado pequeño, puede detectar caras que no hay.
- minNeighbors: Define el número mínimo de vecinos para que un objeto sea considerado una cara, a mayor el número, menos lecturas falsas pero mayor probabilidad de perder lecturas reales.

Además, es conveniente no detectar varias veces la misma cara.

Para este propósito, si hubieramos tenido información intrínseca de la cámara, se podría haber calculado la relación entre los píxeles y las coordenadas en el mundo, sin embargo, a falta de estos datos, esa solución no funciona correctamente. 

En su defecto, usando la posición del robot, y usando un umbral, se puede evitar detectar la misma cara muchas veces.

Como veremos en el video, a veces puede fallar, pero aún así el rendimiento en base a la simplicidad de la solución es muy bueno.

## Movimiento del dron :

El control de movimiento de un dron suele ser algo bastante complejo, sin embargo, gracias a la abstracción de unibotics, el control se hace mucho más sencillo.

Aunque da la posibilidad de controlarlo mediante velocidad, el que mejor desempeño demuestra es el control por posición, en el cual le indicas una posición en coordenadas de Gazebo y el dron se dirije hacia allí.

En esta práctica esta es la única forma de movimiento empleada. Aparte del takeOff, usado para despegar.

## Barrido de la zona :

Como se menciona anteriormente, para esta práctica se indica la posición aproximada de los náufragos, desde la cual el dron debe iniciar una búsqueda para encontrar a todas las personas.

Una primera idea fue la de hacer una espiral creciente, sin embargo, ya que no se usa el control del dron por velocidad, hacer este algoritmo por posición se complica bastante.

Por lo tanto, el método elegido es un barrido de una zona rectangular, cuyo centro aproximado es el punto inicial. 

Esta zona rectangular tiene límites a ambos lados según el tamaño especificado, por lo que se mueve en el eje X de un borde al otro, mientras que va descendiendo en el eje Y poco a poco haciendo un Zig-Zag y cubriendo gran parte de la zona.

Cada vez que el dron llega a un extremo horizontal, se mueve en el eje Y 2 metros. Esto permite que el robot pase muy cerca de zonas ya visitadas para detectar a personas que haya podido perderse en la anterior visita.

Una vez terminado el recorrido, vuelve a empezar para asegurarse que ninguna persona queda sin detectar.

## Paso de coordenadas de Gazebo a UTM

Las coordenadas de las personas son dadas tanto en coordenadas de Gazebo como en UTM, esta conversión se hace sumando 430492 al eje X de las coordenadas de Gazebo, y sumando 4459162 a las del eje Y. Estos números han sido hallados usando la localización en GPS proporcionada en el enunciado.

## Video de demonstración :


<video src="../../assets/img/posts/rescue-dron/video-dron-rescate.webm" controls="controls" style="max-width: 800px;">
</video>