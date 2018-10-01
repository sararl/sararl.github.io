---
layout: post
title: Popups appear over a marker on mouse-over 
tags: [python, folium, markers,popup,geographical information]
#image: /img/hello_world.jpeg
---
For what we have learned a popup marker appear when we click on it. But is is possible the popup appears over a marker on mouse-over by modifiying the html code. I found the solution thanks to *Duc Vu* and this question in stackoverflow  <https://stackoverflow.com/questions/50614427/popup-appear-on-mouse-over-in-folium> . 

```python
import folium
import re
import fileinput

# empty map
m = folium.Map(location=[40.418290, -3.694726], tiles='Stamen Toner', zoom_start=13)
# add markers
folium.Marker(location=[40.407308, -3.690185], popup='Atocha').add_to(m)
folium.Marker(location=[40.416416, -3.703636], popup='Sol').add_to(m)

m.save("MapMadrid_editpopup_mouseon.html")

# open html file and edit
with open("MapMadrid_editpopup_mouseon.html") as inf:
    txt = inf.read()

# Look for the markers in the html code
markers = re.findall(r'\bmarker_\w+', txt)
markers = list(set(markers))

# Edit the lines on the html files with info related to the markers you included.
for markers_sel in markers:
    # inplace=1 means that the file is moved to a backup file and standard output is directed to the input file (if a file # of the same name as the backup file already exists, it will be replaced silently)
    for linenum,line in enumerate( fileinput.input("MapMadrid_editpopup_mouseon.html",inplace=1) ):
        
       pattern = markers_sel + ".bindPopup"
       pattern2 = markers_sel + ".on('mouseover', function (e) {this.openPopup();});"
       pattern3 = markers_sel + ".on('mouseout', function (e) {this.closePopup();});"
    
       if pattern in line:
          print(line.rstrip())
          print(pattern2)
          print(pattern3)
       else:
          print(line.rstrip())     
```
<iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="100%"
    height="400"
    src="/assets/2018-10-01-folium/MapaMadrid_editpopup_mouseon.html">
</iframe>