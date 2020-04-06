---
layout: default
title: Getting started
parent: MiniLibX
grand_parent: Libs
nav_order: 3
---

# Getting started
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Introduction

Now that you know what MiniLibX is capable of doing, we will get started with
doing some very basic things. These will provide you with a solid
understanding of how to write performant code using this library. For a lot of
projects, performance is of the essence, it is therefore of utmost importance
that you read through this section thoroughly.

## Compilation on MacOS

Because MiniLibX requires Appkit and X11 we need to link them accordingly. This
can cause a complicated compilation process. A basic compilation process looks
as follows:

For object files, you could add the following rule to your makefile, assuming
that you have the `mlx` source in a directory named `mlx` in the root of your
project:

```
%.o: %.c
	$(CC) -Wall -Wextra -Werror -Imlx -c $< -o $@
```

To link with the required internal MacOS API's:

```
$(NAME): $(OBJ)
    $(CC) -Lmlx -lmlx -framework OpenGL -framework AppKit -o $(NAME)
```

Do mind that you need the `libmlx.dylib` in the same directory as your build
target as it is a dynamic library!

## Compilation on Linux

In case of Linux, you can use the Codam provided zip which is a linux
compatible MLX version. It has the exact same functions and shares the same
function calls. Do mind, that using memory magic on images can differ as object
implementations are architecture specific. Next, you should unzip the MLX
for linux in a new folder, in the root of your project, called `mlx_linux`.

MiniLibX for Linux requires XOrg, X11 and zlib, therefore you will need to
install the following dependencies: `xorg`, `libxext-dev` and `zlib1g-dev`.
Installing these dependencies on ubuntu can be done as follows:
`sudo apt-get update && sudo apt-get install xorg libxext-dev zlib1g-dev`. Now
all thats left is to configure MLX, just run the `configure` script in the
root of the given repository, and you are good to go.

For object files, you could add the following rule to your makefile, assuming
that you have the `mlx` for linux source in a directory named `mlx_linux` in the
root of your project:

```
%.o: %.c
	$(CC) -Wall -Wextra -Werror -I/usr/include -Imlx_linux -O3 -c $< -o $@
```

To link with the required internal Linux API's:

```
$(NAME): $(OBJ)
    $(CC) -Lmlx_linux -lmlx_linux -L/usr/lib -Imlx_linux -lXext -lX11 -lm -lz -o $(NAME)
```

## Getting a screen on WSL(2)

If you want to get a screen on WSL, follow the following steps:
1. Install [xming](https://sourceforge.net/projects/xming/), just keep clicking
next, the defaults will do. After installing, you will see a little Xming icon
in your icon tray. Now exit xming, and open `XLaunch`, now click the following
in setup:
	- Click multiple windows and go to the next page
	- Click start a program and go to the next page
	- Make sure that the `No Access Control` box is ticked and go to the next page
	- Click save configuration and then finish
2. In WSL execute the following shell script, this will set your display
environment variable accordingly (feel free to create an alias :D):
```sh
export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0.0
```
3. Now you can run graphical applications by calling them from your command line
interface.


## Initialization

Before we can do anything with the MiniLibX library we must include the
`<mlx.h>` header to access all the functions and we should execute the
`mlx_init` function. This will establish a connection to the correct graphical
system and will return a `void *` which holds the location of our current MLX
instance. To initialize MiniLibX one could do the following:

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
window which will stay open forever. You can close it by pressing `CTRL+C` in
your terminal. To achieve this, we will simply call the `mlx_new_window`
function, which will return a pointer to the window we have just created. We can
give the window a height, width and a title. We then will have to call
`mlx_loop` to initiate the window rendering. Let's create a window with a width
of 1920, a height of 1080 and a name of "Hello world!":

```c
#include <mlx.h>

int     main(void)
{
    void    *mlx;
    void    *mlx_win;

    mlx = mlx_init();
    mlx_win = mlx_new_window(mlx, 1920, 1080, "Hello world!");
    mlx_loop(mlx);
}       
```

## Writing pixels to a image

Now that we have basic window management, we can get started with pushing pixels
to the window. How you decide to get these pixels is up to you, however, some
optimized ways of doing this will be discussed. First of all, we should take
into account that the `mlx_pixel_put` function is very, very slow. This is
because it tries to push the pixel instantly to the window (without waiting
for the frame to be entirely rendered). Because of this sole reason, we will
have to buffer all of our pixels to a image, which we will then push to the
window. All of this sounds very complicated, but trust me, its not too bad.

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
pixels to this?  For this we need to get the memory address on which we will
mutate the bytes accordingly. We retrieve this address as follows:

```c
#include <mlx.h>

typedef struct  s_data {
    void        *img;
    char        *addr;
    int         bits_per_pixel;
    int         line_length;
    int         endian;
}               t_data;

int     main(void)
{
    void    *mlx;
    t_data  img;

    mlx = mlx_init();
    img.img = mlx_new_image(mlx, 1920, 1080);

    /*
    ** After creating an image, we can call `mlx_get_data_addr`, we pass
    ** `bits_per_pixel`, `line_length`, and `endian` by reference. These will
    ** then be set accordingly for the *current* data address.
    */
    img.addr = mlx_get_data_addr(img.img, &img.bits_per_pixel, &img.line_length,
                                 &img.endian);
}
```

Notice how we pass the `bits_per_pixel`, `line_length` and `endian` variables
by reference? These will be set accordingly by MiniLibX as per described above.

Now we have the image address, but still no pixels. Before we start with this,
we must understand that the bytes are not aligned, this means that the
`line_length` differs from the actual window width. We therefore should ALWAYS
calculate the memory offset using the line length set by `mlx_get_data_addr`.

We can calculate it very easily by using the following formula:

```c
int     offset = (y * line_length + x * (bits_per_pixel / 8));
```

Now that we know where to write, it becomes very easy to write a function that
will mimic the behaviour of `mlx_pixel_put` but will simply be many times
faster:

```c
typedef struct  s_data {
    void        *img;
    char        *addr;
    int         bits_per_pixel;
    int         line_length;
    int         endian;
}               t_data;

void            my_mlx_pixel_put(t_data *data, int x, int y, int color)
{
    char    *dst;

    dst = data->addr + (y * data->line_length + x * (data->bits_per_pixel / 8));
    *(unsigned int*)dst = color;
}
```

Note that this will cause an issue. Because an image is represented in real time
in a window, changing the same image will cause a bunch of screen-tearing when
writing to it. You should therefore create two or more images to hold your
frames temporarily. You can then write to a temporary image, so that you don't
have to write to the currently presented image.

## Pushing images to a window

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
    t_data  img;

    mlx = mlx_init();
    mlx_win = mlx_new_window(mlx, 1920, 1080, "Hello world!");
    img.img = mlx_new_image(mlx, 1920, 1080);
    img.addr = mlx_get_data_addr(img.img, &img.bits_per_pixel, &img.line_length,
                                 &img.endian);
    my_mlx_pixel_put(&img, 5, 5, 0x00FF0000);
    mlx_put_image_to_window(mlx, mlx_win, img.img, 0, 0);
    mlx_loop(mlx);
}
```

Note that 0x00FF0000 is the hex representation of ARGB(0,255,0,0)

## Test your skills!

Now you that you understand the basics, get comfortable with the library and do
some funky stuff! Here are a few ideas:
- Print squares, circles, triangles and hexagons on the screen by writing the
pixels accordingly;
- Try adding gradients, making rainbows, and get comfortable with using the rgb
colors;
- Try making textures by generating the image in loops.