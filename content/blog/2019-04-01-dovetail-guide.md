---
title: "Dovetail Guide"
date: 2019-04-01
draft: false

# post thumb
image: "images/post/dovetail_guide.png"

# meta description
description: "3d printed parametric dovetail sawing guide for woodworking designed in cadquery"

# taxonomies
categories: 
  - "Computer Aided Design"
tags:
  - "cadquery"
  - "python"
  - "3D printing"
  - "CAD"
 
# post type
type: "post" 
---

This is my take on dovetail guides, I designed this to be 3d printed and parametric. Using parametric design makes it very easy to tune the dovetail ratio and to fit different magnets that one may have at hand. Higher ratios (e.g 1:8) are good for hard woods and lower ratios (e.g. 1:5) are better suited for soft woods. 

I was inspired by [David Barron][1] and as seen is his videos, the simple jig works with magnets that holds the saw in position. This makes the dovetail cutting process easier for woodworking hobbyists like me.

A 45° jig can also be easily made by using a ratio of 1:2. However the script crashes if you use an integer or "2.0" because you don't have a rectangle anymore but an edge. A quick walk around is to use a float like "2.0001". Not very elegant but gets the job done. 

```python
import cadquery as cq

# A simple dovetail saw guide for woodworking 

size = 45.0                      # mm, size of the blank 
ratio = 6.0                      # dovetail ratio e.g. 1:6
tab_top = 15.0                   # mm, thickness of the top tab
tab_back = 15.0                  # mm, thickness of the back tab
magnet_diameter = 10.2           # mm
magnet_thickness = 10.0          # mm 
magnet_spacing_between = 24.0    # mm

dovetail_guide = cq.Workplane("top").rect(size, size).workplane(offset=size).rect(size-(size/ratio)*2, size).loft(combine=True) .\
    faces(">X").workplane().pushPoints([(-magnet_spacing_between/2, size/2-tab_top/2), (magnet_spacing_between/2, size/2-tab_top/2)]).hole(magnet_diameter, magnet_thickness) .\
    faces("<X").workplane().pushPoints([(size/2-tab_back/2, magnet_spacing_between/2), (size/2-tab_back/2, -magnet_spacing_between/2)]).hole(magnet_diameter, magnet_thickness) .\
    faces(">Y").workplane().transformed(offset=(0, -tab_top, 0)).rect(size, size).cutBlind(-size+tab_back)

show_object(dovetail_guide)

```

##### UPDATE (23/04/2020)

Being lock down by the corona pandemic I took the time to improve things a bit. Now all the cadquery scripts on the website should be compatible with CQ 2.0.

I also took the time to modify the script in order to create a right angle sawing guide. This one is inteded to aid making square cuts on wood with hand saws.

Recently I got myself a Gyokucho 615 and I could test it with the jig. No need to say it works like a charm. The cut was square and the texture of the end grain after the cut was very smooth. What a saw, btw! 

I put 3 holes there so it fits right and left handed woodworkers.

```python
import cadquery as cq

# A simple woodworking jig for guiding a saw at right angles.  

size = 50.0                     # mm, size of the blank
width = 35.0                    # mm
tab_top = 18.0                  # mm, thickness of the top tab
tab_back = 12.0                 # mm, thickness of the back tab
magnet_diameter = 10.2          # mm
magnet_thickness = 10.0         # mm 
magnet_spacing_between = 30.0   # mm

right_angle_guide = cq.Workplane("top").box(width, size, size).\
    faces(">X").workplane().pushPoints([(-magnet_spacing_between/2, size/2-tab_top/2), (magnet_spacing_between/2, size/2-tab_top/2), (-size/2+tab_back/2, -magnet_spacing_between/2)]).hole(magnet_diameter, magnet_thickness) .\
    faces(">Y").workplane().transformed(offset=(0, -tab_top, 0)).rect(size, size).cutBlind(-size+tab_back)

show_object(right_angle_guide)
```

Even though this is a simple design I think it is good example of how cool cadquery is. A few lines of human readable code and you have an useful parametric model ready for 3d printing or cnc machining. Iterate the design is fast and easy. 

Thanks for looking

Edi

[1]: http://www.davidbarronfurniture.co.uk/ "David Barron website"
