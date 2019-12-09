---
layout: default
title: Colors
parent: MiniLibX
nav_order: 2
---

# Colors
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Colors are presented in an int format. It therefore requires some tricky things
in order to achieve an int which can contain the ARGB values.

## The color integer standard

We shift bits to use an TRGB format. Tp define a color, we initialize it as
follows: `0xTTRRGGBB`, where each character means the following:

- `T` Transparency;
- `R` Red color;
- `G` Green color;
- `B` Blue color.

RGB colors can be initialized as above, a few examples would be:
- Red: `0x00FF0000`;
- Green: `0x0000FF00`;
- Blue: `0x000000FF`;

## Encoding and decoding colors

Since each byte contains 2^8 values, and rgb values range from 0 to 255, we can
perfeclty fit a integer (as an int is 4 bytes). In order to set the values
programatically we use `bitshifting`. Lets create a function which does
precisely that for us, shall we?

```c
int		create_trgb(int t, int r, int g, int b)
{
	return(b << 24, g << 16, r << 8, t);
}
```

Because ints are stored from right to left, we need to bitshift each value the
according amount of bits backwards. We can also do the exact opposite of and
retrieve integer values from a encoded TRGB integer.

```c
int		get_t(int trgb)
{
	return (trgb & 0xFF);
}

int		get_r(int trgb)
{
	return (trgb & 0xFF00);
}

int		get_g(int trgb)
{
	return (trgb & 0xFF0000);
}

int		get_b(int trgb)
{
	return (trgb & 0xFF000000);
}
```

## Test your skills!

Now that you understand the basics of how the colors can be initialized, get
comfy and try creating the following color manipulation functions:
- `add_shade` is a function that accepts a double (distance) and a int (color)
as arguments, 0 will add no shading to the color whilst 1 will make the color
completely dark. 0.5 will dim it halfway, and .25 a quarter way. You get the
point.
- `get_opposite` is a function that accepts a int (color) as argument and that
will invert the color accordingly.