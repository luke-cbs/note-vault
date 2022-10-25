## Default Container Commands

The default container command is the *command* that gets executed when you run your container.

![[Pasted image 20221025125940.png]]
So when we have a running container the above circles command is what would have veen run to start that containers process. This typically runs as **PID 1**.

These commands can be found in the relevant docker file in that image.

### Change default container command

1)  Change the docker file in the image itself
2)  Run override on container run
   
```
docker container run -d {image_name} {command}
```

## Restart Policies

By default docker containers will not start when they *exit* or when the daemon is *restarted*.

Docker provides us with something called restart polocies. Basically we tell it when it should restart.
This is useful for when your server restarts and you need your dopcker container to restart.

There are four flags for policies:
https://docs.docker.com/config/containers/start-containers-automatically/

To run a new container with a policy use the above to configure it. If you need to change an existing containers polixy run the following:

```
docker update --restart {policy} {container}
```

## Disk Metrics

Similar to [[Useful Commands#Disk Usage]] we can also see the metrics for our docker container using `df`

```
docker system df
```

Will then give us metrics on our images and containers.

```
docker system df -v
```

Will then allow you to see the metrics per component and it will order it from highest to lowest.

## Automatic Container Deletion

Sometimes you may want to have a container exit automatically when it is done. This can be configured with the `--rm` flag.

```
docker container run -dt --rm --name {container_name} {image_name} {command}
```

When this completes it will the remove the container automatically for you.