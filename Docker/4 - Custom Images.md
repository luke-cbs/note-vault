In order to create a custom image you first need to create a docker file with the instructions to do so.

A basic example of an NGINX docker file looks like:


```Dockerfile
# DockerFile
FROM ubuntu
RUN apt-get update
RUN apt-get -y install nginx
CMD ["nginx", "-g", "daemon off;"]
```

## Breaking Down Dockerfile

`FROM`  is the base image we want to use for our image. In the above case we would use Ubuntu

Thereafter are the commands that we want to execute in our Dockerfile.

### Build

Once you are happy with your Dockerfile you can then run

```shell
docker build {dockerfile_path}
```

This will then be added to your images once done.

You can then run your image in a container by using the image ID and running through [[1 - Basics#Container]]