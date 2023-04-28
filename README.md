# osp_on_k8s
This projects aims to evaluate the deployment of OSP on top of K8s

## Environment setup

The setup of a lab environment requires the following:

* A VM with 4x vCPUs and 16 GB of RAM at least to deploy the K8s cluster.
* The deployment of a K8s cluster. I will use Kind as deployment tool.
* The **kubectl** client installed in the VM.

## Installation procedure

1- Update the system to latest version

```
$ sudo dnf -y update
$ sudo reboot
```

2-Install the requirements:

```
$ sudo dnf -y install git podman
```

3-Install kind

```
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.17.0/kind-linux-amd64
chmod +x ./kind
mv ./kind /bin/kind
```

4-Create the K8s cluster:

```
cat <<EOF | kind create cluster --config=-
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: InitConfiguration
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP
  - containerPort: 443
    hostPort: 443
    protocol: TCP
EOF
```

5- Install kubectl

```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
install -o root -g root -m 0755 kubectl /bin/kubectl
```
