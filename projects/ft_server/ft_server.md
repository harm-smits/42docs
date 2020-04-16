---
layout: default
nav_order: 3
title: ft_server
permalink: projects/ft_server
parent: Projects
has_children: false
---
# ft_server

This will be your first project with Docker. Docker is a way of creating
pre-configured virtual machine images that can launch instantly. Docker these
days is used across the globe by many large corporations (Microsoft, Google and
Amazon to name a few).

## Why docker?

There are 2 main reasons why someone would use docker images:
1. Imagine you have a software company and everyone uses their own Linux
distribution or simply a different OS like Mac or Windows, this means that the
code might work on their computer, but not on a production server. Docker was
made to unify the developer experience, so that the code will work no matter
on which operating system you are.
2. Docker images start up really fast, and big corporations usually have
thousands of servers, so quickly updating your software becomes quite a hassle.
Therefore they use Docker images to run their code, as it allows them turn them
off, download a new one, and run it again. This is also known as rolling
updates. Because of this sole reason, they can more easily add servers and
deploy their software to them, as it just needs to download a tiny image and run
it, after which it will work accordingly.

## A very thorough video

[![Image splashscreen](https://img.youtube.com/vi/eGz9DS-aIeY/0.jpg)](https://www.youtube.com/watch?v=eGz9DS-aIeY)

## Prerequisites

Now that you know the usecase for Docker, you must start your project. There are
a few requirements that are not listed in the subject that you should know of:
- Your Docker container must configure / download / install all the services
when you run the `docker build` command, you ought not configure ANY services or
change ANY files after using `docker run` as this mitigates the entire point of
Docker.
- This means wordpress MUST be installed during `docker build` and not by means
of wp_setup.php after running your container.
- Wordpress should work in its entirety, make sure you check for mail posting /
uploading files / uploading themes / installing themes / creating blog posts /
installing modules. You should also verify whether you have the correct php
extensions installed.
- PHPMyAdmin should work in its entirty, so make sure you can create a database,
download a database, create users, remove users, update tables, etc.
- Make sure you have the CORRECT file permissions across your system. This is
INSANELY important as it is the main thing that can guarantee that your docker
container will not become the victim of e.g. remote code execution.

## Getting started

Now that you know what to do, you can get started on Docker. I recommend to use
the following guides / resources:

- [Docker's own guide](https://docs.docker.com/engine/docker-overview/)
- [Debian buster package list](https://packages.debian.org/buster/allpackages)
- [WP Cli to install and configure WordPress](https://github.com/wp-cli/wp-cli)
- [WordPress nginx configuration](https://www.nginx.com/resources/wiki/start/topics/recipes/wordpress/)