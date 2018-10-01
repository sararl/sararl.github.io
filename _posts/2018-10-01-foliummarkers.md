---
layout: post
title: Add markers to your folium map
tags: [python, folium, markers,icon, fontawesome,geographical information]
#image: /img/hello_world.jpeg
---

Let's describe how to add markers to your folium map. It is done using the *folium.Marker* function


### Add simple markers

```python
import folium
# empty map
map_Madrid = folium.Map(location=[40.418290, -3.694726], 
                         zoom_start=12,
                         tiles = "Stamen Terrain")
# create a marker                        
atocha = [40.407308, -3.690185]
markerTrain = folium.Marker(location = atocha, 
                            popup="Madrid-Puerta de Atocha")# pop up is the label for the marker
# add marker to the map
markerTrain.add_to(map_Madrid)
map_Madrid
```
<iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="100%"
    height="400"
    src="/assets/2019-10-01-folium/simpleMarker.html">
</iframe>


You can edit the icon using *folium.Icon* class. These objects allow you to change the colour of your markers using *color* and *icon_color* parameters. The parameter *color* defines the color of the marker and you can use one of these values: 'red', 'blue', 'green', 'purple', 'orange', 'darkred', 'lightred', 'beige', 'darkblue', 'darkgreen', 'cadetblue', 'darkpurple', 'white', 'pink', 'lightblue', 'lightgreen','gray', 'black', 'lightgray'. The *icon_color* parameter is the color of the drawing on the marker, you can use colors above or an html color code.


```python
# create the icon
icon_feature = folium.Icon(color="red",icon_color="blue")
# create a new marker
chamartin = [40.471142, -3.680689]
marcador = folium.Marker(location = chamartin
                        , popup="Madrid-Charmartin"
                        ,icon = icon_feature)
# add marker to the map
marcador.add_to(map_Madrid)
map_Madrid
```
<iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="100%"
    height="400"
    src="/assets/2019-10-01-folium/simpleMarker2.html">
</iframe>

### Change the drawing on the marker

#### New icon

Instead of using the the drawing by default ("info-sign") you can use one of the icons in this website: <https://fontawesome.com/>. Be careful, font awesome fonts build under version 5 don't render in folium 0.5.0. 


```python
# empty map
map_Madrid = folium.Map(location=[40.418290, -3.694726], 
                         zoom_start=12)
# define icon
iconTrain = folium.Icon(prefix = "fa", # because the source is fontawesome
                        icon = "train", # name of the icon
                        icon_color = "#E40EF2", 
                        color = "black") 
# define marker
atocha = [40.407308, -3.690185]
marc = folium.Marker(location = atocha, popup="Madrid-Puerta de Atocha",
              icon=iconTrain)
# add marker to the map
marc.add_to(map_Madrid)
map_Madrid

```
<iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="100%"
    height="400"
    src="/assets/2019-10-01-folium/ChangeDrawing.html">
</iframe>

#### An image

In addition you can create a custom icon based on an image by using objects of class *folium.features.CustomIcon*.

```python
# empty map
map_Madrid = folium.Map(location=[40.418290, -3.694726], 
                         zoom_start=12)

# define icon
# icon image
image = 'https://upload.wikimedia.org/wikipedia/commons/thumb/2/2f/Adif_wordmark.svg/512px-Adif_wordmark.svg.png'

iconLogo = folium.features.CustomIcon(icon_image= image,icon_size=(50,50))
# define marker
markeratocha = folium.Marker(location = [40.407308, -3.690185]
                            , popup="Madrid-Puerta de Atocha"
                            , icon=iconLogo)
# add marker
markeratocha.add_to(map_Madrid)
mapa_Madrid

```
<iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="100%"
    height="400"
    src="/assets/2019-10-01-folium/imageMap.html">
</iframe>

### BeautifyIcon

The last versions of folium present an extra plugin which offers special icons. You can edit not only the colors, but also the shape, and the border width of the icon. 

```python
from folium.plugins.beautify_icon import BeautifyIcon
# empty map
map_Madrid = folium.Map(location=[40.418290, -3.694726], 
                         zoom_start=12)
# define icon                         
icon_train = BeautifyIcon(
    icon='train',
    border_color='#b3334f',
    border_width =2,
    text_color='#E40EF2',
    icon_shape='circle')
# define marker
marker_train = folium.Marker(location= atochaCoordenadas
                            , popup="Madrid-Puerta de Atocha"
                            , icon=icon_train)
# add marker
marker_train.add_to(map_Madrid)
map_Madrid


```
<iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="100%"
    height="400"
    src="/assets/2019-10-01-folium/BeautifyIcon.html">
</iframe>

