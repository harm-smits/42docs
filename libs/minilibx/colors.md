---
layout: default
---

# Colors

Colors are presented in an int format. We shift bits to use an TRGB format. To
define a color, we initialized it as follows: `0xTTRRGGBB`, where each character
means the following:

- `T` Transparency;
- `R` Red color;
- `G` Green color;
- `B` Blue color.

RGB colors can be initialized as above, a few examples would be:
- Red: `0x00FF0000`;
- Green: `0x0000FF00`;
- Blue: `0x000000FF`;

## Test your skills!

Now that you understand the basics of how the colors can be initialized, get
comfy and try creating the following color manipulation functions:
- `add_shade` is a function that accepts a double (distance) and a int (color)
as arguments, 0 will add no shading to the color whilst 1 will make the color
completely dark. 0.5 will dim it halfway, and .25 a quarter way. You get the
point.
- `get_opposite` is a function that accepts a int (color) as argument and that
will invert the color accordingly.