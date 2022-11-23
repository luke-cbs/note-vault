When refferring to a node we are reffering to an instance of the Docker engine participating in the swarm. To deploy your application to a swarm you would submit a service definition to a manger node.

## Creating Manager Node

The manager node is any one of the nodes (VMs) that you decide. The manager node dispatches units of work called tasks to worker nodes.

```sh
docker swarm init --advertise-addr {manager_ip})
```

Once this is run it will give you back a command something to the key of:

```sh
 docker swarm join --token {token}
```

![[Pasted image 20221122165448.png]]
When your workers have joined the swarm you should see something along these lines.

## Services, Tasks and Containers

To see all current services use `service ls`.

A service is the definition of the tasks to execute on the manager or worker nodes.

```sh
docker service create --name webserver --replicas 1 nginx
```

With the above script we are creating a service called `webserver` this will start up a task per replica (**Containers in a service are called tasks**). using the nginx image.

Once create your can then use `docker service ps {service_name}` to then see your service and get more details on what and where it is running.

If you have run the above and then ran `docker ps` you will be able to see your container running. If you had to stop that container then run `docker service ps {service_name}` again you can then see that it has started up a new nginx container.

This is because our service will ensure that we always have 1 nginx replica always running as we specified. It will decide which swarm node it will run on. It may not always be the same node.

#### Remove service

To remove a serivice use:
```sh
docker service rm {service_name}
```

When doing this it will also shut down all running contianers on the relevant nodes.

## Scaling Service in Swarm

Once you have deployed a service to a sawrm you can use the Docker CLI to scale the number of containers.

So if you have create your service as per above you may want to scale your service up or down.

There are two approaches and both be used to then create more tasks based on the service replica originally set. This is both for upscaling and dowscaling.

##### Approach 1

```sh
docker service scale {service_name}={amount}
```

##### Approach 2

```sh
docker service update --replicas {amount} {service_name}
```

The difference between the 2 approaches is with the first command we can actually specify multiple services at the same time. Where as approach 2 can only do a specific service.

## Replicated vs Global Service

A replicated service, you specify the number of identical tasks you want to run. For example if you want to deploy and nginx service with 2 replicas each serving same content.

**Example**

```sh
docker service create --name replicaservice --replicas 1 nginx
```

A global service is a service that runs one task on every node. Each time you add a node to the swarm, the orchestrator creates a task and the scheduler assings the task to the new node.

**Example**

```sh
docker service create --name globalservice --mode global -dt ubuntu
```

Now the the key difference here is we are passing a mode and specifying global and in doing so we are telling this to run this service across all of the nodes in that swarm. You can confirm this by running `docker service ps globalservice`.

## Draining Swarm Node

A good use case for this is you may have a monthly life cycle and want to update one of the nodesin a swarm or some form of maintenence. 

To take a node out you need drain it so that the tasks are distributed to the other nodes.

```sh
docker node update --availability drain {node}
```

Run `docker node ls` to see the availability.

So if we now had to scale this service since the node has been flagged as drained it will not add new tasks to that specific node.

To make this no longer the case we need to mark the node as active.

```sh
docker node update --availability active {node}
```

This will not re-distribute tasks to the now active node.

## Inspecting Swarm Services and Nodes

In order to get detailed information about a service:

```sh
docker service inspect {service_name}
```

You can then use the `--pretty` flag to make it more readable.

You can then inspect and find out information about a node in a swarm as well.

```sh
docker node inspect {node} --pretty
```

## Adding Network and Publishing Ports to Swarm Tasks

If for example you run the following

```sh
docker service create --name myweb --replicas 2 nginx
```

This will not publish the ports for that nginx webserver on those nodes.

In order to do this create your new service but we will specify the port mappings:

```sh
docker service create --name myweb --replicas 2 --publish 8080:80 nginx
```