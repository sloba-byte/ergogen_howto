# Ergogen how-to

![keys](keys.jpeg "Keys")

Based on [ergogen](https://github.com/ergogen/ergogen) and flatfootfox tutorials:\
[units](https://flatfootfox.com/ergogen-part1-units-points/)\
[outlines](https://flatfootfox.com/ergogen-part2-outlines/)\
[cases_3d](https://flatfootfox.com/ergogen-part4-footprints-cases/)\
Still to cover pcb:\
[pcbs](https://flatfootfox.com/ergogen-part3-pcbs/) & [finale](https://flatfootfox.com/ergogen-part5-kicad-firmware-assembly/)

Visualization while building yml config is better done on [unofficial](https://ergogen.cache.works/) web tool.

## [Absolem](https://zealot.hu/absolem/)

Best to start with simplified Absolem keyboard (found in web tool)

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
# spread (in +-x direction)
  ks: kx + 0.2
# padding (in +-y direction)
  kp: ky + 0.2
# switch dimensions x and y
  sx: 14
  sy: 14
  
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
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/0e/Cartesian-coordinate-system.svg/1280px-Cartesian-coordinate-system.svg.png" alt="coordinate system">

Everything so far connected together.
## **Keys positions**
```yml
units:
# keycap dimension
  kx: 18
  ky: 18
# spread/padding
  ks: kx + 0.2
  kp: ky + 0.2
# switch dimension
  sx: 14
  sy: 14
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