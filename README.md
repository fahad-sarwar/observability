# observability

https://github.com/jaegertracing/jaeger-operator

# Jaeger Operator for Kubernetes

The Jaeger Operator is an implementation of a [Kubernetes Operator](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/).

## Getting started

Firstly, ensure an [ingress-controller is deployed](https://kubernetes.github.io/ingress-nginx/deploy/). When using `minikube`, you can use the `ingress` add-on: `minikube start --addons=ingress`

To install the operator, run:
```
kubectl create namespace observability
kubectl create -n observability -f deploy/jaegertracing.io_jaegers_crd.yaml
kubectl create -n observability -f deploy/service_account.yaml
kubectl create -n observability -f deploy/role.yaml
kubectl create -n observability -f deploy/role_binding.yaml
kubectl create -n observability -f deploy/operator.yaml
```

The operator will activate extra features if given cluster-wide permissions. To enable that, run:
```
kubectl create -f deploy/cluster_role.yaml
kubectl create -f deploy/cluster_role_binding.yaml
```

Note that you'll need to download and customize the `cluster_role_binding.yaml` if you are using a namespace other than `observability`. You probably also want to download and customize the `operator.yaml`, setting the env var `WATCH_NAMESPACE` to have an empty value, so that it can watch for instances across all namespaces.

Once the `jaeger-operator` deployment in the namespace `observability` is ready, create a Jaeger instance, like:

```
kubectl apply -n observability -f deploy/jaeger-operator.yaml
```

This will create a Jaeger instance named `simplest`. The Jaeger UI is served via the `Ingress`, like:

```console
$ kubectl get -n observability ingress
NAME             HOSTS     ADDRESS          PORTS     AGE
simplest-query   *         192.168.122.34   80        3m
```

In this example, the Jaeger UI is available at http://192.168.122.34.
