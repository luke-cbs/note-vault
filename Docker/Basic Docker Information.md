## Docker Images vs Containers

### Image

This is a file which contains the dependencies and configurations to run an application.

To find existing images you can visit: https://hub.docker.com/

To pull down an image:

```shell
docker pull {image}
```

To confirm available images:

```shell
docker images
```

### Container

##### Running a container
This is basically the running instance of an image.

To run a docker container with an image:

```shell
docker run --name {container_name} -dt -p 80:80 {image_id_or_name}
```

##### Port breakdown
By default your containers are not open for the outside world to look into. To do so you have to specify the port you want to expose external. So doing `8000:80` the **LHS (8000)** will be the **host** request will send to the **RHS (80)** which is a container.

To see a docker containers internal IP you can use

```shell
docker inspect {image_name}
```

This will then show you a lot of details but at the bottom you can see:
![[Pasted image 20221019165145.png]]

You cannot connect to this private network from outisde of your container.

##### Container Status and Commands

To see currently running containers: 

```shell
docker ps
```

To then stop a docker container:

```shell
docker stop {docker_image_name_or_id}
```

This is if you want to see all docker containers regardless if they are running.

```shell
docker ps -a
```

This means you are then able to get the name and then restart your container with:

```shell
docker start {name}
```

If you are unsure the public IP you can run:

```shell
ifconfig
```

Once your container is running again you are able to also use `netstat -ntlp` to verify that your docker container is running on the specified port:
![[Pasted image 20221019162008.png]]
As well as running a `curl {ip}` to verify the http response and that it is running as expected.