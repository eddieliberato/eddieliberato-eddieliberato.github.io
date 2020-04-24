---
title: "Kugelwellenhorn"
date: 2018-08-19
draft: false

# post thumb
image: "images/post/horn_cad.png"

# meta description
description: "parametric horn for 3d print designed in cadquery"

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

I always have been intrigued by horns. Acctualy by passive devices in general, tunned mass dampers, RLC circuits to name a few. Researching more about horns I found two designs that I'd like to explore more. One is the Kugelwellenhorn and the other is the Le Cleah'c horn. More info about the former can be found [here][1], in a lecture apparently gaven by Le Cleach'c himself when he was still alive.

About Kugellwellen horn, the design that I decided to approach first, all the info you need you can probably find in the site of [Bjorn Kolbrek][2]. Mr. Kolbrek seens to be an expert on subject, and I enjoyed a lot reading his articles. The math on his reports was a bit beyond my comprehension and I didn't want to dive so deep into the subject. My goal was only to design and materialize the horn. Turns out that this is not hard at all, Just take a look at the script below and you'll see that with about 30 lines of code is done. Amazing!. Well, again hats off to cadquery developers that make it possible.


```python
import cadquery as cq
import math
import matplotlib.pyplot as plt

c = 344000    # mm/s speed of sound through air
fc = 480      # Hz cuttof frequency
td = 12       # mm throat diameter
steps = 450   # related to the length of the horn
m = 4*math.pi*fc/c
y0 = td/2
r0 = c/(math.pi*fc)
h0 = r0 - math.sqrt(math.pow(r0, 2)-math.pow(y0, 2))
s0 = 2*math.pi*r0*h0
edg_points = []

for x in range(steps):
    h = h0*math.exp(m*x)
    s = 2*math.pi*r0*h
    y = math.sqrt((s/math.pi)-math.pow(h, 2))
    xh = x-h+h0
    edg_points.append([y, xh])

edg_points[0] = [y0, 0]
fig, ax = plt.subplots()
ax.plot(list(zip(*edg_points))[1], list(zip(*edg_points))[0])
ax.set(xlabel='length (mm)', ylabel='contour (mm)',
       title='Kugelwellen horn profile')
ax.grid()
plt.show()

horn = cq.Workplane("XY").move(y0-0.01,0).spline(edg_points).wire().revolve()

show_object(horn, options={"rgba":(130, 185, 255, 0.0)})
```

One thing that's interesting to notice, Mr. Lecleach suggests that it's important to shape the horn past 180 degrees but as you can see it can be tricky to 3d print the involute part of the horn. Would be interesting to come up with some 3d printing hack to print the involute. Maybe one day I come back to this topic to try something in this direction.  

##### UPDATE (23/04/2020)

Being lock down by the corona pandemic I took the time to improve things a bit. Now all the cadquery scripts on the website should be compatible with CQ 2.0.

Looking the script above that I wrote almost two years ago it's easy to understand why it crashes in CQ 2.0. It's a bit of a mess with this calling of matplotlib library. I must say it was convinient to look at the profile of the horn and to create the 3d model at the same time, but it's ugly code. 

I was looking at the script to generate cycloid gears from [Adam Urbanczyk][3], one of the developers of cadquery, and what an elegant code! This inspired me not only to remove this plotting part of the script, but also to modify it a bit in order to make it more elegant. 

```python
import cadquery as cq
from math import pi, sqrt, exp
 
td = 12      # mm throat diameter
steps = 400  # related to the length of the horn
fc = 480     # Hz cuttof frequency

c = 344000   # mm/s speed of sound through air

m = 4*pi*fc/c
y0 = td/2
r0 = c/(pi*fc)
h0 = r0 - sqrt(r0*r0-y0*y0)
s0 = 2*pi*r0*h0
edg_points = []

for x in range(steps):
    h = h0*exp(m*x)
    s = 2*pi*r0*h
    y = sqrt((s/pi)-h*h)
    xh = x-h+h0
    edg_points.append([y, xh])

edg_points[0] = [y0, 0]

horn = cq.Workplane("XY").move(y0, 0).spline(edg_points).wire().revolve()

show_object(horn)
```

Much better isn't it ?

Thanks for looking

Edi

[1]: http://www.rintelen.ch/download/JMMLC_horns_lecture_etf10.pdf "lecleach"
[2]: http://kolbrek.hoyttalerdesign.no/index.php/horns/horn-theory "kolbrek horn theory"
[3]: https://github.com/adam-urbanczyk
