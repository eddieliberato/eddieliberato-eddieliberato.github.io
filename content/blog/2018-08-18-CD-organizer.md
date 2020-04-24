---
title: "CD organizer"
date: 2018-08-18
draft: false

# post thumb
image: "images/post/cd_organizer.png"

# meta description
description: "CD organizer model for 3D print. Designed in cadquery using python list comprehension to create a beatiful parametric model"

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

I had some old CDs lying around the house and I decided to organized them in a single place. I thought of 3d printing something to help, but after a brief search around the internet I couldn't find anything that I liked. The ones I could find were big and clunky, leading to long printing times. The "simple" one (that wasn't simple at all) that I found had the slots a bit undersized and I couldn't fit the CDs. I could have rescaled the model but that print took me around 3 hours and I started to wonder if I should repeat it. 

I decided to come up with my own design.


```python
import cadquery as cq

slots = 8               # number slots (number of CDs in this case)
slot_width = 11.0       # mm, slot size (slightly bigger than a CD case width) 
slot_depth = 20.0       # mm
slot_length = 140.0     # mm
thickness_wall = 0.8    # mm
thickness_bottom = 2.0  # mm

spacing = slot_width + thickness_wall

cdorg = cq.Workplane("front").box(slot_length+thickness_wall, spacing*slots+thickness_wall, slot_depth+thickness_bottom) .\
    faces(">Z").workplane().center(thickness_wall,-(spacing/2*(slots-1))).pushPoints([(0, y * spacing) for y in range(0, slots)]).rect(slot_length+thickness_wall, slot_width).cutBlind(-slot_depth).\
    faces(">Z").rect(slot_length/1.6, spacing*slots+thickness_wall).cutBlind(-slot_depth)

show_object(cdorg)
```

The print took only 14 minutes cause I could take advantage of the 0.8mm nozzle and print the walls in a single pass of the hotend. The design turned out to be not super visually appealing but the function was on point. It keeps the CDs more accessible than just piling them up and it makes everything more stable against tipping over.

More important than the physical object itself was that I learned something. I find this method of using the for loop (python's list comprehension) inside the pushPoints method very elegant and I plan to reuse this code for lathe tools and drill bits organizers among others.

##### UPDATE (23/04/2020)

Being lock down by the corona pandemic I took the time to improve things a bit. Now all the cadquery scripts on the website should be compatible with CQ 2.0.

Thanks for looking

Edi 
