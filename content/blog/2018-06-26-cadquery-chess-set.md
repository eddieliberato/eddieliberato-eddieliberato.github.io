---
title: "Cadquery Chess Set"
date: 2018-06-26
draft: false

# post thumb
image: "images/post/chessfreecad.png"

# meta description
description: "A chess set designed in cadquery. 3d printing project"

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

This is my debute 3d printing project. A chess set designed in cadquery. 

```python
# Chess set
import cadquery as cq

# Chess board squares size
cbss = 25
right_side_up_pawns = True    # False is probably better for 3d printing

# king base diameter and height
kb = cbss*0.75
kh = kb*2.5

king = cq.Workplane("front").circle(kb/2).workplane(offset=kh*0.7).circle(kb*0.8/2).loft(combine=True).\
    faces(">Z").workplane().circle(kb*0.8/2).workplane(offset=kh*0.1).circle(kb*0.4/2).loft(combine=True).\
    faces(">Z").workplane().circle(kb*0.4/2).workplane(offset=kh*0.08).circle(kb*0.7/2).loft(combine=True).\
    faces(">Z").workplane().circle(kb*0.7/2).workplane(offset=kh*0.12).polygon(4, kb).loft(combine=True).\
    faces(">Z").hole(kb*0.75,kh*0.03).translate((0,cbss*3,0))


queen = cq.Workplane("front").circle(kb/2).workplane(offset=kh*0.5).circle(kb*0.6/2).loft(combine=True). \
    faces(">Z").workplane().circle(kb*0.6/2).workplane(offset=kh*0.12).circle(kb/2).loft(combine=True). \
    faces(">Z").workplane().circle(kb/2).workplane(offset=kh*0.1).circle(kb*0.4/2).loft(combine=True). \
    faces(">Z").workplane().circle(kb*0.4/2).workplane(offset=kh*0.1).circle(kb*0.9/2).loft(combine=True). \
    faces(">Z").hole(kb*0.6,kh*0.03) .\
    faces(">Z[-2]").workplane().circle(kb*0.4/2).extrude(kh*0.05) .\
    faces(">Z").fillet(kh*0.05-0.1).translate((0,cbss*4,0))


def rook():
    rk = cq.Workplane("front").circle(kb/2).workplane(offset=kh*0.7).polygon(4, kb*1.6).loft(combine=True). \
    faces(">Z").hole(kb*1.2,kh*0.05)
    return rk


def knight():
    mouth = cq.Workplane("top").polygon(4, kb*0.8).extrude(kb*2).translate((kb*0.75,-kb,kb*1.3))
    kn = cq.Workplane("front").circle(kb/2).workplane(offset=kh*0.7).center(-kb*0.1,0).polygon(2, kb*1.5).loft(combine=True)
    kn = kn.cut(mouth)
    return kn


def bishop():
    hat = cq.Workplane("front").circle(kb*0.6/2).workplane(offset=kh*0.2).polygon(4,kb*1.2).loft(combine=True). \
        faces(">Z").center(-kb*0.2,0).cskHole(kb*0.4,kb,72).translate((0,0,kh*0.5))
    bi = cq.Workplane("front").circle(kb/2).workplane(offset=kh*0.5).circle(kb*0.6/2).loft(combine=True)
    bi = bi.add(hat).combine()
    return bi


def pawn():
    pa = cq.Workplane("front").circle(kb/2).extrude(kh*0.1) .\
        faces(">Z").workplane().circle(kb*0.4/2).workplane(offset=kh*0.25).circle(kb/2).loft(combine=True) .\
        edges("not>Z").fillet(kb*0.1)
    if right_side_up_pawns:
        pa = pa.rotate((0,0,0), (1,0,0),180).translate((0,0,kh*0.35))
    return pa


Lrook = rook()
Rrook = rook().translate((0, cbss*7, 0))

Lknight = knight().translate((0, cbss, 0))
Rknight = knight().translate((0, cbss*6, 0))

Lbishop = bishop().translate((0, cbss*2, 0))
Rbishop = bishop().translate((0, cbss*5, 0))

Lrook_pawn = pawn().translate((cbss, 0, 0))
Lknight_pawn = pawn().translate((cbss, cbss, 0))
Lbishop_pawn = pawn().translate((cbss, cbss*2, 0))
king_pawn = pawn().translate((cbss, cbss*3, 0))
queen_pawn = pawn().translate((cbss, cbss*4, 0))
Rbishop_pawn = pawn().translate((cbss, cbss*5, 0))
Rknight_pawn = pawn().translate((cbss, cbss*6, 0))
Rrook_pawn = pawn().translate((cbss, cbss*7, 0))

chess_set = king.add(queen).add(Lrook).add(Rrook).add(Lknight).add(Rknight) .\
    add(Lbishop).add(Rbishop).add(Lrook_pawn).add(Lknight_pawn) .\
    add(Lbishop_pawn).add(king_pawn).add(queen_pawn).add(Rbishop_pawn) .\
    add(Rknight_pawn).add(Rrook_pawn).combine()


#show

#show_object(Lrook, options={"rgba":(204, 204, 204, 0.0)})
#show_object(Lknight, options={"rgba":(204, 204, 204, 0.0)})
#show_object(Lbishop, options={"rgba":(204, 204, 204, 0.0)})
#show_object(queen, options={"rgba":(204, 204, 204, 0.0)})
#show_object(king, options={"rgba":(204, 204, 204, 0.0)})
#show_object(Rbishop, options={"rgba":(204, 204, 204, 0.0)})
#show_object(Rknight, options={"rgba":(204, 204, 204, 0.0)})
#show_object(Rrook, options={"rgba":(204, 204, 204, 0.0)})
#show_object(Lrook_pawn, options={"rgba":(204, 204, 204, 0.0)})
#show_object(Lknight_pawn, options={"rgba":(204, 204, 204, 0.0)})
#show_object(Lbishop_pawn, options={"rgba":(204, 204, 204, 0.0)})
#show_object(king_pawn, options={"rgba":(204, 204, 204, 0.0)})
#show_object(queen_pawn, options={"rgba":(204, 204, 204, 0.0)})
#show_object(Rbishop_pawn, options={"rgba":(204, 204, 204, 0.0)})
#show_object(Rknight_pawn, options={"rgba":(204, 204, 204, 0.0)})
#show_object(Rrook_pawn, options={"rgba":(204, 204, 204, 0.0)})
show_object(chess_set, options={"rgba":(130, 185, 255, 0.0)})
```

Some final comments about the script: I read an article [here][1] to more or less have some guidance about the parameters. The diameter of the bottom of the pieces should be 75% of the playing square and the height should be 2.5 x that number. They call this the 75% guideline. 

It's a bit unusual object to draw using parametric CAD and probably easier with point and click softwares but it was an interesting parametric exercise. I had a lot of fun coding the pieces and printing them.


[1]: https://www.chessusa.com/chess-pieces-size.html "chess pieces size"


Thanks for looking

Edi 
