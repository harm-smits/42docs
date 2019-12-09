---
layout: default
title: Hooks
parent: MiniLibX
nav_order: 4
---

# Hooks
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

In computer programming, the term hooking covers a range of techniques used to
alter or augment the behaviour of an operating system, of applications, or of
other software components by intercepting function calls or messages or events
passed between software components. Code that handles such intercepted function
calls, events or messages is called a hook.

Hooking is used for many purposes, including debugging and extending
functionality. Examples might include intercepting keyboard or mouse event
messages before they reach an application, or intercepting operating system
calls in order to monitor behavior or modify the function of an application or
other component. It is also widely used in benchmarking programs, for example
frame rate measuring in 3D games, where the output and input is done through
hooking.

Simply put, it is therefore not weird that hooking is the backbone of MiniLibX.

## Hooking example

Hooking may sound difficult, but it really is not. Let's take a look shall
we?

```c
#include <mlx.h>

typedef struct  s_vars {
    void        *mlx;
    void        *win;
}               t_vars;

int             close(int keycode, t_vars *vars)
{
    mlx_destroy_window(vars.mlx, vars.mlx_win);
}

int             main(void)
{
    t_vars      vars;

    vars.mlx = mlx_init();
    vars.mlx_win = mlx_new_window(mlx, 1920, 1080, "Hello world!");
    mlx_key_hook(vars.mlx_win, close, &vars);
} 
```

We have now registered a function that will close the window whenever we press
a key. As you can see, we register a hook function with `mlx_key_hook`, however,
in the background, it simply calls the function `mlx_hook` with the appropriate
X11 event types. We will discuss this in the next chapter.

## Test your skills!

Now that you have a faint idea of what hooks are, we will allow you to create a
few of your own. Create hook handlers that whenever:
- a key is pressed, it will print the key code in the terminal.
- the mouse if moved, it will print the current position of that mouse in the
terminal.
- a mouse is pressed, it will print the angle at which it moved over the window
to the terminal.


