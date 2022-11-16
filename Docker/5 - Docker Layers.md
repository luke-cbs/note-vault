## What are layers?

Docker images are built from a series of layers:

```docker
FROM ubuntu:15.04
COPY . /app
RUN make /app
CMD python /app/app.py
```

Each of the above can be considered as layers that translate to from bottom to top something along the lines of:

```
[
	Layer 4 [132135vmsfda 188 0 B] - CMD
	Layer 3 [15535vmsgfgd 188 1.8 KB] - RUN
	Layer 2 [1251aa3vmsfa 195 KB] - COPY
	Layer 1 [125135vmsfda 188 MB] - ubuntu
]
```

The above losely gives us an idea of how things are split within an image. It is important to note that when creating layers that your understand how each layer is adding to your image size. 

For example by creating multiple layers that could be done in a single layer you could unecassarily be adding extra MB to your image.

In order to see the history of a docker image or rather what it is made up of and its layers:

```sh
docker image history {image}
```

### Docker Containers and Layers

When it comes to images that are built these are considered READ-ONLY. 

What this means is you can have multiple containers that run off the same image and these make modifications to how they are run. However they are not changing the underlying image.
