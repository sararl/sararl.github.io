---
layout: post
title: Edit the popups of your folium markers
tags: [python, folium, markers,icon, popup,geographical information]
#image: /img/hello_world.jpeg
---

In the previous post we learned how to add markers to our folium maps. If you remember we include a *popup* argument when we create the marker. This argument was a string variable that was the marker label. In this post we're going to learn how to edit this popup, and show tables, images or even a website when you click on the marker.

### A table inside the popup

```python
import folium
import pandas as pd
# Empty map
m = folium.Map([40.407308, -3.690185],zoom_start = 12)

# Define the dataframe you want to show in the popup
df = pd.DataFrame(data = [["5:00-00:00","7:00-21:00","9:00-20:00"],["7:00-00:00","9:00-20:00","9:00-15:00"]],
                  columns = ["Horario de la estación","Horario de venta de billetes","Horario atención cliente"],
                  index = ["De lunes a viernes","Sábados, domingos y festivos"])
# Transform the dataframe to html code
htmldf = df.to_html()
# define the marker
markerTable = folium.Marker(location = [40.407308, -3.690185]
                            , popup = htmldf)
# add the marker to the map
markerTable.add_to(m)

m
```
<iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="100%"
    height="400"
    src="/assets/2018-10-01-folium/MapaAtocha_PopUptabla.html">
</iframe>

### Customize the html code inside the popup

You can put any HTML code inside of a popup.

```python
# empty map
m = folium.Map(location=[40.418290, -3.694726], tiles='Stamen Toner', zoom_start=13)

# html code that will be include inside the popup
htmlcode = """
    <h2 style="font-family:verdana;">  Estación de atocha </h2>
    <p>
    La estación de Atocha es un complejo ferroviario situado en las cercanías de la plaza del
    Emperador Carlos V, en Madrid, España. Hace las funciones de nudo ferroviario, y esto la 
    convierte en la estación con más tráfico de pasajeros del país.<br><br>
    </p>
    <p>
    <h1>Información de horarios</h1>
    <table style="width:80%">
      <tr>
        <th> Horario de la estación </th>
        <th> Venta inmediata de billetes </th> 
        
      </tr>
      <tr>
        <td>DIARIO: 05:00 A 01:00</td>
        <td>DIARIO: 06:15 A 22:15</td>
        
      </tr>
      </table>
    </p>
    Click <a href="http://www.adif.es/es_ES/infraestructuras/estaciones/60000/informacion_000070.shtml">aqui</a> para tener más información.    
    """ 
# create the marker
markerEdit = folium.Marker(location=[40.407308, -3.690185]
                            , popup= htmlcode)

# add marker
markerEdit.add_to(m)

m
```
<iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="100%"
    height="400"
    src="/assets/2018-10-01-folium/MapaAtocha_PopUpConEstilo.html">
</iframe>

### Include images and websites inside the popups

```python
# empty map
m = folium.Map(location=[40.418290, -3.694726], tiles='Stamen Toner', zoom_start=13)

# include a website in the html code
htmlatocha = """<iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="300"
    height="200"
    src="http://www.adif.es/es_ES/infraestructuras/estaciones/60000/informacion_000070.shtml">
</iframe>
"""
# include an image
htmlsol = """<img src="https://www.spain.info/export/sites/spaininfo/comun/carrusel-recursos/madrid/puerta-del-sol-madrid-71220719-istock.jpg_369272544.jpg" alt="Flowers in Chania"
width="300" height="200" alt ="Foto sol">
"""
# create and include markers
folium.Marker(location=[40.407308, -3.690185], popup=htmlatocha).add_to(m)
folium.Marker(location=[40.416416, -3.703636], popup=htmlsol).add_to(m)

m
```
<iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="100%"
    height="400"
    src="/assets/2018-10-01-folium/MapaAtocha_FotosWeb.html">
</iframe>

### A map inside the popup!

You can include a map in the popup! Is it useful? I don't think so... but it's cool!

```python
import branca
# two empty maps
map_1 = folium.Map([40.416416, -3.703636], zoom_start=10)
map_2 = folium.Map([41.501526, 2.165353], zoom_start=10)

# add a marker in map2
folium.Marker(location=[41.501526, 2.165353], popup='previous location').add_to(map_2)

# figure with a map inside
fig = branca.element.Figure()
map_2.add_to(fig)
# iframe with the figure inside 
iframe = branca.element.IFrame(width=300, height=300)
fig.add_to(iframe)
# iframe inside popup
popup = folium.Popup(iframe)

# add a markers in map1 
folium.Marker([40.416416, -3.703636], popup=popup).add_to(map_1)

map_1
```
<iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="100%"
    height="400"
    src="/assets/2018-10-01-folium/MapaDentrodeMapa.html">
</iframe>


In a future post I show you different ways to include a plot inside a popup marker.

