# docker-bg

An attempt of Blue Green deployment using Docker.

# About

This POC is powered by the following tools:

* Fig: a tool used to manage an application in distributed containers.
> See: http://www.fig.sh/

* Nginx: an HTTP and reverse proxy server.
> See: http://nginx.org/

* Consul: a tool for discovering and configuring services in your infrastructure.
> See: https://www.consul.io/

* Consul-template: a daemon used to populate values from Consul on your filesystem.
> See: https://github.com/hashicorp/consul-template

* Registrator: a tool that automatically register/deregister Docker containers into Consul.
> See: https://github.com/gliderlabs/registrator

# How to

## Pre-requisites

Ensure you have Docker installed.

Install fig:

````
$ sudo pip install -U fig
````

Update the *fig.yml* file and replace *ROUTABLE_IP* with a routable IP address (use your main interface IP address).

Build the 2 sites images:

````
$ docker build -t sitea siteA/
$ docker build -t siteb siteB/
````

## Let's play

Start the Nginx + Consul + Registrator stack:

````
$ fig pull & fig build
$ fig up -d
````

Start a "v1" container:

````
$ docker run -d -e "SERVICE_NAME=my_service" -e "SERVICE_TAGS=my_tag" -p 80 -d sitea
````

Point your browser at http://localhost to see the result.

Next, start a "v2" container:

````
$ docker run -d -e "SERVICE_NAME=my_service" -e "SERVICE_TAGS=my_tag" -p 80 -d siteb
````

Point your browser at http://localhost to see the result, you should now be load balanced between the 2 versions.

Stop your v1 container and enjoy a smooth BG deployment :)

## Bonus

Access the Consul UI via http://localhost:8500

