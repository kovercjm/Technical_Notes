# 0 Introduction

Istio is a completely open source service mesh that layers transparently onto existing distributed applications. It is also a platform, including APIs that let it integrate into any logging platform, or telemetry or policy system.

The following content is summarized from [Istio Official Documentation](https://istio.io/latest/).

# 1 Binary Client

Install command line runnable on Mac:

```shell
# via HomeBrew
brew install istioctl

# via GitHub release
ISTIO_VERSION=1.9.2
curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istioctl-$ISTIO_VERSION-osx.tar.gz" | tar xz
sudo mv ./istioctl /usr/local/bin/istioctl
sudo chmod +x /usr/local/bin/istioctl
```

Enable Kubernetes in Docker Desktop or connect to cloud K8S service.

# 2 Installation

Install Istio to Kubernetes:

```shell
# using istioctl
istioctl install

# using Istio Operator (with more secure)
istioctl operator init
kubectl create ns istio-system
kubectl apply -f - <<EOF
apiVersion: install.istio.io/v1alpha1
kind: IstioOperator
metadata:
  namespace: istio-system
  name: istio-control-plane
spec:
  profile: demo
EOF

```

If just wants to play with the official demo sample:

```shell
curl -L https://istio.io/downloadIstio | sh -
cd istio-$ISTIO_VERSION
export PATH=$PWD/bin:$PATH
istioctl install --set profile=demo -y
```

Enable Istio auto-inject sidecar proxy when deployment:

``` sh
kubectl label namespace default istio-injection=enabled
```

Download [official addons](https://github.com/istio/istio/tree/master/samples/addons) for analysis dashboards, and enable:

``` shell
kubectl apply -f addons/
```
