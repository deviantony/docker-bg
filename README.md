# docker-bg

An attempt of Blue Green deployment using Docker.


# How to

## Pre-requisites

Update the *fig.yml* file and replace <ROUTABLE_IP> with a routable IP address (use your IP address).

Build the 2 sites images:

````
$ docker build -t sitea siteA/
$ docker build -t siteb siteB/
````

## Let's play

Start the Nginx + Consul + Registrator stack:

````
$ fig up -d
````

Start a "v1" container:

````
$ docker run -d -e "SERVICE_NAME=app" -e "SERVICE_TAGS=production" -p 80 -d sitea
````

Point your browser at http://localhost to see the result.

Next, start a "v2" container:

````
$ docker run -d -e "SERVICE_NAME=app" -e "SERVICE_TAGS=production" -p 80 -d siteb
````

Point your browser at http://localhost to see the result, you should now be load balanced between the 2 versions.

Stop your v1 container and enjoy a smooth BG deployment :)
