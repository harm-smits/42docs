---
layout: default
nav_order: 4
title: ft_services
permalink: projects/ft_services
parent: Projects
has_children: false
---

# ft_services

Make a kubernetes cluster that will make sure your wordpress installatio
conforms to [High Availability](https://en.wikipedia.org/wiki/High_availability)
by means of services.

## Why kubernetes?

Kubernetes is a portable, extensible, open-source platform for managing
[containerized applications](https://www.ibm.com/cloud/learn/containerization)
and services that facilitates both declarative configuration and automation.
Kubernetes provides a platform to configure, automate, and manage:

- Intelligent and balanced scheduling of containers
- Creation, deletion, and movement of containers
- Easy scaling of containers
- Monitoring and self-healing abilities

Major compnies use Kubernetes to achieve their goals, on of those companies is
Faceit, a major esports company providing high performance servers for
competitive online games like CSGO. Read more about how they use Kubernetes
[here](https://cloud.google.com/customers/faceit/).

## Prerequisites

- If any service crashes in a docker image, you must make sure it gets replaced
accordingly, you can do this by looking into:
    1. [Supervisor](https://blog.trifork.com/2014/03/11/using-supervisor-with-docker-to-manage-processes-supporting-image-inheritance/)
    which provides you with powerful configuration options to make sure that your
    docker image will exit if a service stops responding.
    2. [Health checks](https://blog.couchbase.com/docker-health-check-keeping-containers-healthy/)
    which provide you with a very easy way to check if your service is still up
    and running.
- All docker images MUST be build manually. No including of external images.
- Everything must be up and running and persist when needed. This includes, but
is not limited to: your wordpress `wp-content` folder, your FTPS data, the
influxdb data file, the grafana database with settings, and the mysql data file.

## Getting started

Because you can't use an Elastic Block Store service on the iMacs at school, you
will require some sort of a VM to provide support for this. One of such
solutions is [Minikube](https://kubernetes.io/docs/setup/learning-environment/minikube/).
This tool is 100% recommended for setting up your Kubernetes cluster.

The [kubernetes documentation](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
is not that good but it will get you a long way if you want to get started, so
its definitely recommended to get a bit of information about it. After that you
might want to get started with [installing wordpress](https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/)
followed with the [installation of influxdb, grafana and telegraf](https://octoperf.com/blog/2019/09/19/kraken-kubernetes-influxdb-grafana-telegraf/).
