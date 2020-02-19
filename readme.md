# Kubernetes Example Controller

An example controller in go which prints the number of replicas when a deployment is scaled. Local dev environment with [kind](https://kind.sigs.k8s.io/) and [ko](https://github.com/google/ko).

## setup dev environment
Make sure Docker is up and running. Then start a local Kubernetes cluster with [kind](https://kind.sigs.k8s.io/):
```bash
./kind-with-registry.sh
```

This will start a docker registry and a Kubernetes cluster and wires them up. Easy peasy right?

## build and deploy the controller
Our source is written in go and thus we can use [ko](https://github.com/google/ko) to build and deploy:
```bash
export KO_DOCKER_REPO=localhost:5000/controller-example
ko apply -f deployment.yaml
```

## use

scale a deployment
```bash
kubectl logs -f controller-example-7f7fffd5c5-pdnml
kubectl scale deployment --replicas 2 controller-example
kubectl scale deployment --replicas 4 controller-example
```
watch the controller logs
```bash
2020/02/15 14:16:12 Deployment: controller-example, Replicas: 2
2020/02/15 14:16:19 Deployment: controller-example, Replicas: 4
```