Kubernetes (k8s) is an orchestration engine by Google.

Similar to Docker swarm master node. Kubernetes would create a master node with various worker nodes.

There are 3 ways to install K8s:

- Managed K8s service (Digital Ocean, AWS, etc.)
- Minikube (Does not provide everything)
- Manual Installation (Harder)

## Installing kubectl

https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

### Helpful Refs

```sh
kubectl api-resources

kubectl explain {resource}
```

## Connecting to K8 Master

To connect to the K8s master there are 2 things needed:

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

## Kubernetes Objects

Pods are an example of an Object. There are other instances such as namespace, deployments and so on, they are not just pods.

There are various ways to configure an Object.

- `kubectl`
- Config file in YAML

### Example POD YAML

To create run `kubectl apply -f {filename}.yaml`
To delete run `kubectl delete -f {filename}.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginxwebserver
spec:
  containers:
  -  image: nginx
     name: democontainer
```

## Labels and Selectors

Labels are key balue pairs atached to objects such as pods. They are great for identifying and running commands based on labels.

```
name: gateway
env: production
```

With the above we can see an example of 2 labels. With a key of name/env and value of gateway/production.