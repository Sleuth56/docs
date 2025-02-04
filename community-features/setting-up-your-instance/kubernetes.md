# Kubernetes

## Prerequisites

To deploy and use Defguard on your cluster you'll need:

* a [Kubernetes cluster](https://kubernetes.io/docs/setup/)
* Kubernetes CLI [kubectl](https://kubernetes.io/docs/reference/kubectl/) installed on your machine
* helm binary https://github.com/helm/helm/releases/latest

## Deployment

We prepared a [git repository](https://github.com/DefGuard/deployment) with Kubernetes configuration, clone it:

```
git clone https://github.com/DefGuard/deployment.git && cd deployment/charts
```

Then create namespace for Defguard on your cluster:

```
kubectl create namespace defguard
```

Copy and fill in values file:

```
cp defguard/values.yaml ./
```

Required values (the rest should work if left as-is):

* `ingress.hosts.grpc`: GRPC ingress address - grpc clients like defguard-gateway, youbi-bridge etc. will connect here.
* `ingress.hosts.web`: Web ingress address - Defguard webapp will be available here.
* `publicUrl`: Public URL your Defguard will be available under. Usually the same as ingress.hosts.web, but differ depending on your loadbalancer and/or reverse-proxy setup.

If you want to deploy the enrollment service along with your Defguard instance you also need to configure values related to the `defguard-prox`subchart:&#x20;

* `defguard-proxy.enabled`: enable the enrollment service
* `defguard-proxy.upstreamGrpcUrl`: defguard server gRPC endpoint URL
* `defguard-proxy.publicUrl`: public URL of the enrollment service
* `defguard-proxy.ingress.hosts.web`: enrollment service ingress address

And finally install the Helm chart in the namespace:

* `publicUrl`: Public URL your Defguard will be available under. Usually the same as ingress.hosts.web, but differ depending on your loadbalancer and/or reverse-proxy setup.

```
helm install --wait=true --namespace defguard defguard defguard -f values.yaml
```
