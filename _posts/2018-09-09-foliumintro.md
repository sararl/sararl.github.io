---
layout: post
title: Tu primer mapa con folium
tags: [python, folium, geographical information]
#image: /img/hello_world.jpeg
---

Folium es una librería de Python que permite crear mapas interactivos usando Leaflet.js. Básicamente le damos instrucciones sencillas y javascript hace un montón de trabajo en el fondo que nos permite obtener mapas muy, muy intersantes. Mapas con marcadores, heatmaps, mapas cloropléticos, en varias capas,... ¡Parecerás un auténtico especialista en visualizaciones de mapas con solo escribir unas pocas líneas de código! 

Escribiré varias entradas relacionadas con esta librería. En este primer post haremos simplemente la visualización de un mapa centrado en las coordenadas que le indiquemos.

```python
import folium
mapa_malaga = folium.Map(location=[36.715090, -4.461963],zoom_start= 12) 
# location: tupla o lista [latitud,longitud] que indica donde estará el mapa centrado.
# zoom_start: cuanto más grande sea el valor que tome más próximo estará el mapa.
mapa_malaga
```
<iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="100%"
    height="400"
    src="/assets/2018-09-09-folium/MapaMalaga.html">
</iframe>

Si lo quieres guardar en un archivo html:

```python
mapa_malaga.save("yourpath/nombreMapa.html")
```

Podemos modificar el mapa de base que se visualizará. Prueba con distintos valores de *tiles*.

```python
# posibles valores
listatiles = ["OpenStreetMap",  # valor por defecto
              "Mapbox Bright", "Mapbox Control Room", # estos dos limitan los niveles de zoom
              "Stamen Terrain", "Stamen Toner", "Stamen Watercolor"]

mapa_malaga = folium.Map(location=[36.715090, -4.461963],tiles ="Stamen Toner", zoom_start= 14) 
mapa_malaga
```

<iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="100%"
    height="400"
    src="/assets/2018-09-09-folium/MapaMalaga_personalizado1.html">
</iframe>


Puedes definir también:
* El tamaño máximo de zoom permitido ("max_zoom",int, por defecto = 18 )
* El tamaño del mapa ("width" y "height" , int o percentage string, por defecto ="100%")
* La aparición de un referente de escala ("control_scale", boolean, por defecto = False)
* La repetición del mapa ("no_wrap" , boolean, por defecto = False)

Como siempre recomiendo echar un ojo a la ayuda, para recordar, entender o aprender como tenemos que pasar cada parámetro:

```python
help(folium.Map)
```
Aquí por ejemplo hacemos uso de los parámetros que controlan el tamaño del mapa, o la repetición del mapa.

```python
mapa_mundial = folium.Map(location=[36.715090, -4.461963],tiles ="Stamen Terrain",zoom_start= 1,
               max_zoom = 5, width='90%',height='50%',control_scale=True,no_wrap=True)
mapa_mundial
```
<iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="100%"
    height="400"
    src="/assets/2018-09-09-folium/MapaMundial_personalizado1.html">
</iframe>

Recordad que esto es solo el principio. A continuación añadiremos marcadores al mapa y alguna funcionalidad a través de plugins.