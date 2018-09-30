---
layout: post
title: Add plugins to your folium map 
tags: [python, folium, plugin,geographical information]
#image: /img/hello_world.jpeg
---

Folium has a package which wraps some of the most popular leaflet external plugins. In this post I will focus on two of the plugins that I find more interesting to add to a map that will be presented or delivered to a client.

Float image plugin adds a floating image in HTML canvas on top of the map. It would be very useful to add your personal information or your company logo ~~or a funny image~~. The current method doesn't allow you to modify the width and height of the image, in a future post I'll show you how to face it.

```python
from folium.plugins import FloatImage
mapa_Malaga_plugin = folium.Map(location=[36.715090, -4.461963], 
                                tiles ="Stamen Toner" ,
                                zoom_start=14)

url = 'https://www.shareicon.net/download/2016/01/01/228426_magic_256x256.png'
#url = "unicorn.jpg"
logo = FloatImage(image = url # path local image or url 
            , bottom=45, left=5) # the origin is the lower left corner
logo.add_to(mapa_Malaga_plugin)

mapa_Malaga_plugin
```
<iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="100%"
    height="400"
    src="/assets/2018-09-10-folium/MapaMalaga_pluginFlotante.html">
</iframe>

Fullscreen plugin adds a fullscreen button to your map. I consider this is a wonderful tool to capture your audience's attention.

*It seems that the fullscreen button doesn't work in the html embebed in the website iframe, but test yourself. It works when you open the html file.*
```python
from folium.plugins import Fullscreen
mapa_Malaga_plugin = folium.Map(location=[36.715090, -4.461963], zoom_start=12)

pluginCompleta = Fullscreen(
                position='topleft',
                title='Change to fullscreen',
                title_cancel='Exit',
            force_separate_button=True)

pluginCompleta.add_to(mapa_Malaga_plugin)
mapa_Malaga_plugin
```
<iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="100%"
    height="400"
    src="/assets/2018-09-10-folium/MapaMalaga_pluginCompleta.html">
</iframe>


I have added the plugins over simple maps, without markers or layers, however these plugins could be added to more complex maps. 

