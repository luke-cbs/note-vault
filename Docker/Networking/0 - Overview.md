Within a docker container you have access to a few different network drivers.

Docker takes care of networking aspects so that containers can communicate with each other as well as the docker host.

There are several drivers available:

- bridge
- host 
- overlay
- macvlan
- none

To see install network drivers use `docker network ls`. You can then get more information on a driver by using `docker inspect {driver_name}`.

To inspect the containers in a network you would use `docker network inspect {network}`.
To then see what driver a container is running use `docker container inspect {container}`.