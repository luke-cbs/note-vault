When refferring to a node we are reffering to an instance of the Docker engine participating in the swarm. To deploy your application to a swarm you would submit a service definition to a manger node.

### Create Manager Node

The manager node is any one of the nodes (VMs) that you decide. The manager node dispatches units of worl called tasks to worker nodes.

```sh
docker swarm init --advertise-addr {manager_ip})
```

Once this is run it will give you back a command something to the key of:

```sh
 docker swarm join --token {token}
```

![[Pasted image 20221122165448.png]]
When your workers have joined the swarm you should see something along these lines.