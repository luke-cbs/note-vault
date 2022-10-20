Docker container exec commands run a new command in a running container.

This means that if you have a container that is active you can execute commands inside of that container.

For example:

```shell
docker container exec -it {container_name} bash
```

Will allow you to run any binary that is within your container. So above we are execiting bash. Which then means we are able to run and see our NGINX status.

```shell
/etc/init.d/nginx status
```

To then exit your container either `ctrl + d` or type `exit`.
