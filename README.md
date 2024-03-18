# Ergogen how-to

![keys](./resources/keys.jpeg "Keys")

Based on [ergogen](https://github.com/ergogen/ergogen) and flatfootfox tutorials:\
[units](https://flatfootfox.com/ergogen-part1-units-points/)\
[outlines](https://flatfootfox.com/ergogen-part2-outlines/)\
[cases_3d](https://flatfootfox.com/ergogen-part4-footprints-cases/)\
Still to cover pcb:\
[pcbs](https://flatfootfox.com/ergogen-part3-pcbs/) & [finale](https://flatfootfox.com/ergogen-part5-kicad-firmware-assembly/)

Visualization while building yml config is better done on [unofficial](https://ergogen.cache.works/) web tool.

## [Absolem](https://zealot.hu/absolem/)

Best to start with simplified Absolem keyboard (found in web tool)
<img src="./resources/absolem.svg" alt="absolem layout">

```yml
points:
  zones:
    matrix:
      columns:
        pinky:
        ring:
          key.splay: -5
          key.origin: [-12, -19]
          key.stagger: 12
        middle:
          key.stagger: 5
        index:
          key.stagger: -6
        inner:
          key.stagger: -2
      rows:
        bottom:
        home:
        top:
    thumbfan:
      anchor:
        ref: matrix_inner_bottom
        shift: [-7, -19]
      columns:
        near:
        home:
          key.spread: 21.25
          key.splay: -28
          key.origin: [-11.75, -9]
        far:
          key.spread: 21.25
          key.splay: -28
          key.origin: [-9.5, -9]
      rows:
        thumb:
  rotate: -15
  mirror:
    ref: matrix_pinky_home
    distance: 223.7529778
```
## Details:

### **Matrix** -> Rows & Columns:
```yml
...
matrix:
    columns:
        # so it's 5 colums
        # 4 per each finger 
        # +5th called inner
        pinky:
        ring:
        # change splay/origin/stagger to understand what they do
        key.splay: -5
        key.origin: [-12, -19]
        key.stagger: 12
        middle:
        key.stagger: 5
        index:
        key.stagger: -6
        inner:
        key.stagger: -2
    rows:
        # and it has 3 rows
        bottom:
        home:
        top:
...
```
One can reference positions of matrix 2d points to build shapes via matrix_**column**_**row** ex. "ref: matrix_middle_home"

### **Matrix** -> Thumbfan:

```yml
...
matrix:
...
    thumbfan:
    # here is already example of reference
    # it's saying based on which point thumb fan should be placed
      anchor:
        ref: matrix_inner_bottom
        # "shifting" in 2d space by x, y
        shift: [-7, -19]
    # and then describe thumb fan via matrix
      columns:
      # has 3 columns
        near:
        home:
        #again best to manipulate and visualize this to understand what they do
          key.spread: 21.25
          key.splay: -28
          key.origin: [-11.75, -9]
        far:
          key.spread: 21.25
          key.splay: -28
          key.origin: [-9.5, -9]
      rows:
      # has 1 row
        thumb:
...
```

### Rotate & Mirror
```yml
...
# gives a rotation to half of a keyboard
  rotate: -15
# and then mirror left half to create right
  mirror:
    # mirror around what (again using ref)
    ref: matrix_pinky_home
    # how far (from pinky to pinky)
    distance: 223.7529778
```

## Units
Only unit we used is how far mirroring is done (223.7529778), but still generated keyboard looks good? Ergogen internally specifies default units (in web tool one can examine units.yml and points.yml files).\
Units are in **Millimeters** (if not specified differently)

By default Ergogen offers some units for Chock and Standard MX style switches/keycaps (details can be found at [units](https://flatfootfox.com/ergogen-part1-units-points/))\
This keyboard is based on [gatheron mx low profile switches](https://www.gateron.co/products/gateron-low-profile-mechanical-switch-set)which are a bit different in dimension so Units will be specified from scratch:

```yml
units:
# keycap dimension x and y
  kx: 18
  ky: 18
  pad: 0.2
# spread (in +-x direction)
  ks: kx + pad
# padding (in +-y direction)
  kp: ky + pad
# padding, change this to find right setting for your 3d printer
  sw_pad: 0.2
# switch dimensions x and y
  sx: 14 + sw_pad
  sy: 14 + sw_pad
points:
  zones:
    matrix:
      key:
        # spread is in X direction
        spread: 1ks
        # padding is in Y direction
        padding: 1kp
        # it's possible to do math ops like 1.5ks (1.5 * ks) or any other math op
      columns:
...
```

Cartesian coordinate system:
<img src="./resources/cartesian_system.svg" alt="coordinate system">

Everything so far connected together.
## **Keys positions**
```yml
units:
# keycap dimension
  kx: 18
  ky: 18
# spread/padding
  pad: 0.2
  ks: kx + pad
  kp: ky + pad
# switch dimension
  sw_pad: 0.2
  sx: 14 + sw_pad
  sy: 14 + sw_pad
points:
  zones:
    matrix:
      key:
        spread: 1ks
        padding: 1kp
      columns:
        pinky:
        ring:
          key.splay: -5
          key.origin: [-12, -19]
          key.stagger: 12
        middle:
          key.stagger: 5
        index:
          key.stagger: -6
        inner:
          key.stagger: -2
      rows:
        bottom:
        home:
        top:
    thumbfan:
      anchor:
        ref: matrix_inner_bottom
        shift: [-7, -19]
      columns:
        near:
        home:
          key.spread: 21.25
          key.splay: -28
          key.origin: [-11.75, -9]
        far:
          key.spread: 21.25
          key.splay: -28
          key.origin: [-9.5, -9]
      rows:
        thumb:
  rotate: -15
  mirror:
    ref: matrix_pinky_home
    distance: 223.7529778
```

Some updates to keys position:
- mirror distance:
```diff
mirror:
  ref: matrix_pinky_home
- distance: 223.7529778
+ distance: 284
```
- rotate:
```diff
- rotate: -15
+ rotate: -10
```
- ring 
```diff
ring:
  key.splay: -5
- key.origin: [-12, -19]
+ key.origin: [-12, -16]
- key.stagger: 12
+ key.stagger: 11
```
- thumbfan:
```diff
thumbfan:
       anchor:
         ref: matrix_inner_bottom
-        shift: [-7, -19]
+        shift: [-10, -30]
       columns:
         near:
+          key.splay: -6
         home:
-          key.spread: 21.25
-          key.splay: -28
-          key.origin: [-11.75, -9]
+          key.spread: 19
+          key.splay: -38
+          key.origin: [-10, -9]
         far:
-          key.spread: 21.25
-          key.splay: -28
+          key.spread: 19
+          key.splay: -32
           key.origin: [-9.5, -9]
       rows:
         thumb:
```
P.S. My hands are shifted by 1 column to the outside.\
More natural position for thumbs I would say would be "shift: [-3, -30]" for regular person instead of mine "shift: [-10, -30]".

## **Keys positions with personal changes**
<img src="./resources/keycaps.svg" alt="keycap position">

```yml
units:
# keycap dimension
  kx: 18
  ky: 18
# spread/padding
  pad: 0.2
  ks: kx + pad
  kp: ky + pad
# switch dimension
  sw_pad: 0.2
  sx: 14 + sw_pad
  sy: 14 + sw_pad
points:
  zones:
    matrix:
      key:
        spread: 1ks
        padding: 1kp
      columns:
        pinky:
        ring:
          key.splay: -5
          key.origin: [-12, -16]
          key.stagger: 11
        middle:
          key.stagger: 5
        index:
          key.stagger: -6
        inner:
          key.stagger: -2
      rows:
        bottom:
        home:
        top:
    thumbfan:
      anchor:
        ref: matrix_inner_bottom
        shift: [-10, -30]
      columns:
        near:
          key.splay: -6
        home:
          key.spread: 19
          key.splay: -38
          key.origin: [-10, -9]
        far:
          key.spread: 19
          key.splay: -32
          key.origin: [-9.5, -9]
      rows:
        thumb:
  rotate: -10
  mirror:
    ref: matrix_pinky_home
    distance: 284
```


# Outlines (Shapes)

Next thing to do it to draw some shape around keys. This is basis for building 3d printed case and PCBs later on.
<img src="./resources/switches.svg" alt="switch position">

```yml
...
outlines:
  switches:
    - what: rectangle
      where: true
      size: [sx,sy]
```

How to visualize this in web tool? On right side click preview next to switches.dxf
<img src="./resources/download_web_tool.png" alt="downloads">
