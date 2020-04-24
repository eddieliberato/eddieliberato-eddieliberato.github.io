---
title: "Cadquery Turners Cube"
date: 2018-07-04
draft: false

# post thumb
image: "images/post/turners_cube.png"

# meta description
description: "Parametric 3d model of a turners cube designed in cadquery"

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

Back in Brazil, during my machinist apprenticeship, one of the lathe tasks of the manual machining course was to make a turners cube. Basically you need to turn a cube in the lathe and make some holes, that if carefully bored to specific dimensions, you end up with usually two captive cubes inside one another. At the time I was only a young teenager and the captive cube was a very attractive challenge to me. I naively thought that only round parts could be made at the lathe, so I was impressed with the cube with other cubes inside coming out of the machine. Sadly nowadays I don't know anymore where mine is. Hopefully at my mom's house.

You can have as many cubes as you want inside. Back then,  the ones who actually made the cube usually went for three, so I did and I still think that three is the right number if you're doing it on the manual lathe. Two is too easy. Four is annoying because it is a lot of repetition of the same operations. Three is a good number because you proved your point in showing the instructor what you're capable of, and is not as annoying to make as the four cubes version. 

Leaving 2003 behind and back to 2018, I was thinking that the parametric nature of the object makes it a nice model for cadqquery. So here is the script that I came up with:


```python
import cadquery as cq
from math import sqrt

captive_cube_size = 10    # mm
n_of_cubes = 3            # besides the innermost captive cube
small_holes = True        # False = innermost captive cube without holes

cube_size = captive_cube_size*2**n_of_cubes  # total size of the cube
sh_size = captive_cube_size/sqrt(2)          # diameter of the hole on the smallest cube

cube = cq.Workplane("front").box(cube_size, cube_size, cube_size)
for x in range(n_of_cubes):
    cube = cube.faces(">X").workplane().hole(captive_cube_size*(sqrt(2)**(2*x+1)), (cube_size-captive_cube_size*2**x)/2)
    cube = cube.faces(">Y").workplane().hole(captive_cube_size*(sqrt(2)**(2*x+1)), (cube_size-captive_cube_size*2**x)/2)
    cube = cube.faces(">Z").workplane().hole(captive_cube_size*(sqrt(2)**(2*x+1)), (cube_size-captive_cube_size*2**x)/2)
    cube = cube.faces("<X").workplane().hole(captive_cube_size*(sqrt(2)**(2*x+1)), (cube_size-captive_cube_size*2**x)/2)
    cube = cube.faces("<Y").workplane().hole(captive_cube_size*(sqrt(2)**(2*x+1)), (cube_size-captive_cube_size*2**x)/2)
    cube = cube.faces("<Z").workplane().hole(captive_cube_size*(sqrt(2)**(2*x+1)), (cube_size-captive_cube_size*2**x)/2)

if small_holes:
    cube = cube.faces("<X").workplane().hole(sh_size)
    cube = cube.faces("<Y").workplane().hole(sh_size)
    cube = cube.faces("<Z").workplane().hole(sh_size)

show_object(cube)
```

I based my script in the innermost captive cube and in the square root of 2, which is a number associated with circles and squares. That makes the cube looks good and proportional. The drawback is that the final size grows quadratically with the number of cubes resulting in a big thing if you go for a lot of cubes.

##### UPDATE (23/04/2020)

Being lock down by the corona pandemic I took the time to improve things a bit. Now all the cadquery scripts on the website should be compatible with CQ 2.0.

Thanks for looking

Edi 
