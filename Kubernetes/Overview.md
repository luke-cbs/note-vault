Kubernetes (k8s) is an orchestration engine by Google.

Similar to Docker swarm master node. Kubernetes would create a master node with various worker nodes.

There are 3 ways to install K8s:

- Managed K8s service (Digital Ocean, AWS, etc.)
- Minikube (Does not provide everything)
- Manual Installation (Harder)

## Installing kubectl

https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

## Connecting to K8 Master

To connec to the K8s master there are 2 things needed:

- DNS/IP
- Auth Creds

## Creating Pods

```sh
kubectl run {name} --image={image}
```

### Connecting to a POD

```sh
kubectl exec -it {name} -- bash
```