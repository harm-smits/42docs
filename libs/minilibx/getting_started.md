---
layout: default
title: Getting started
parent: MiniLibX
nav_order: 3
---

# Getting started
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Now that you know what MiniLibX is capable of doing, we will get started with
doing some very basic things. Before we can do anything with the MiniLibX
library we must include the `<mlx.h>` header to access all the functions and
we should execute the `mlx_init` function. This will establish a connection
to the correct graphical system and will return a `void *` which holds the
location of our current MLX instance. To initialize MiniLibX one could do the
following:

```c
#include <mlx.h>

int     main(void)
{
    void    *mlx;

    mlx = mlx_init();
}
```

When you run the code, you can't help but notice that nothing pops up and that
nothing is being rendered. Well, this obviously has something to do with the
fact that you are not creating a window yet, so lets try initializing a tiny
window which will close itself after a few seconds. To achieve this, we will
simply call the `mlx_new_window` function, which will return a pointer to the
window we have just created. We can give the window a height, width and a
title. Here we will create a window with a width of 1920, a height of 1080 and
a name of "Hello world!":

```c
#include <mlx.h>
#include <unistd.h> // for sleep

int     main(void)
{
    void    *mlx;
    void    *mlx_win;

    mlx = mlx_init();
    mlx_win = mlx_new_window(mlx, 1920, 1080, "Hello world!");
    sleep(5);
    mlx_destroy_window(mlx, mlx_win);
}       
```

Now that we have basic window management, we can get started with pushing pixels
to the window. How you decide to get these pixels is up to you, however, some
optimized ways of doing this will be discussed. First of all, we should take
into account that the `mlx_pixel_put` function is very, very slow. This is
because it tries to push it instantly to the window (without waiting for the)
frame to be entirely rendered. Because of this sole reason, we will have to
buffer all of our input to an image, which we will then write to the window. All
of this sounds very complicated, but trust me, its not too bad.

First of all, we should start by understanding what type of image `mlx`
requires. If we initiate an image, we will have to pass a few pointers to which
it will write a few important variables. The first one is the `bpp`, also called
the bits per pixel. As the pixels are basically ints, these usually are 4 bytes,
however, this can differ if we are dealing with a small endian (which means we
most likely are on a remote display and only have 8 bit colors).

Now we can initialize the image with size 1920 x 1080 as follows:

```c
#include <mlx.h>

int     main(void)
{
    void    *img;
    void    *mlx;

    mlx = mlx_init();
    img = mlx_new_image(mlx, 1920, 1080);
}
```

That wasn't too bad, was it? Now, we have an image but how exactly do we write
pixels to this? That is a very good question. For this we need to get the
memory address on which we will mutate the bytes accordingly. We retrieve this
address as follows:

```c
#include <mlx.h>

int     main(void)
{
    void    *img;
    void    *mlx;
    char    *addr;
    int     bits_per_pixel;
    int     line_length;
    int     endian;

    mlx = mlx_init();
    img = mlx_new_image(mlx, 1920, 1080);
    addr = mlx_get_data_addr(img, &bits_per_pixel, &line_length, &endian);
}
```

Notice how we pass the `bits_per_pixel`, `line_length` and `endian` variables
by reference. These will be set accordingly by MiniLibX as per described above.

Now we have the image address, but still no pixels. Before we start with this,
we must understand that the bytes are not aligned, this means that the
`line_length` differs from the actual window width. We therefore should ALWAYS
calculate the memory offset using the line length set by `mlx_get_data_addr`.

We can calculate it very easily by using the following formula:

```c
int     offset = ((y * line_length + x * (bits_per_pixel / 8));
```

Now that we know where to write, it becomes very easy to write a function that
will mimic the behaviour of `mlx_pixel_put`:

```c
typedef struct  s_data {
    void        *img;
    char        *addr;
    int         bits_per_pixel;
    int         line_length;
    int         endian;
}               t_data;

void            my_mlx_pixel_put(t_data data, int x, int y, int color)
{
    char    *dst;

    dst = data.addr + ((y * data.line_length + x * (data.bits_per_pixel / 8));
    *(unsigned int*)dst = *(unsigned int*)color;
}
```

Now that we can finally create our image, we should also push it to the window,
so that we can actually see it. This is pretty straight forward, lets take a
look at how we can write a red pixel at (5,5) and put it to our window:

```c
#include <mlx.h>

typedef struct  s_data {
    void        *img;
    char        *addr;
    int         bits_per_pixel;
    int         line_length;
    int         endian;
}               t_data;

int             main(void)
{
    void    *mlx;
    void    *mlx_win;
    t_data  *img;

    mlx = mlx_init();
    mlx_win = mlx_new_window(mlx, 1920, 1080, "Hello world!");
    img.img = mlx_new_image(mlx, 1920, 1080);
    img.addr = mlx_get_data_addr(img, &img.bits_per_pixel, &img.line_length,
                                 &img.endian);
    my_mlx_pixel_put(data, 5, 5, 0x00FF0000);
    mlx_put_image_to_window(mlx, mlx_win, img.img, 0, 0);
}
```

## Test your skills!

Now you that you understand the basics, get comfortable with the library and do
some funky stuff! Here are a few ideas:
- Print squares, circles, triangles and hexagons on the screen by writing the
pixels accordingly;
- Try adding gradients, making rainbows, and get comfortable with using the rgb
colors;
- Try making textures by generating the image in loops.