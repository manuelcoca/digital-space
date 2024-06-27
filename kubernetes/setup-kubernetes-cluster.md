# Setup Kubernetes Cluster

## Cluster requirements

A node can be configured on a virtual or physical machine. Requirements for a good virtual machine:

* 2 vCPUs
* 8GB RAM
* 20GB Memory
* Dont' use 192.168 network for nodes
* No firewall
* Disable swap (necessary for kubectl to work properly)
* Disable SELinux and AppArmor

Connect to virtual machine via SSH: `ssh <user>@<ip-address>`

### My Lab

Cluster exists of 3 nodes (ubuntu VMs):

* master - `$ ssh -i "master.pem" ubuntu@master`
* worker 1 - `$ ssh -i "master.pem" ubuntu@worker-1`
* worker 2 - `$ ssh -i "master.pem" ubuntu@worker-2`

## Setup Ubuntu (22.04) Master Node from scratch

### Prepare VM

```
$ sudo apt update
$ sudo swapoff -a
$ sudo nano /etc/fstab #comment outswap partition entry to avoid enable swap after reboot
$ sudo hostnamectl set-hostname <node-name>
```

### Set up IPv4 Forwarding and letting iptables see bridged traffic

```
$ cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

$ sudo modprobe overlay
$ sudo modprobe br_netfilter

### sysctl params required by setup, params persist across reboot ###
$ cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

### Apply sysctl params without reboot ###
$ sudo sysctl --system
```

### Installation

```
### Install basic packages ###
$ sudo apt install -y apt-transport-https ca-certificates curl wget git

### Download the Google Cloud public signing key and add the kubernetes apt repository ###
$ curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/k8s.gpg
$ sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"

### Install Kubernetes repositories and Docker ###
$ sudo apt-get update
$ sudo apt-get install -y kubelet kubeadm kubectl docker.io
$ sudo apt-mark hold kubelet kubeadm kubectl docker.io
```

### Cluster Setup

<pre><code>### Set the cgroup driver for runc to systemd required for kubelet
### Matching the container runtime and kubelet cgroup drivers is required or otherwise the kubelet process will fail.
$ sudo mkdir -p /etc/containerd
$ sudo containerd config default | sudo tee /etc/containerd/config.toml
$ sudo sed -i 's/            SystemdCgroup = false/            SystemdCgroup = true/' /etc/containerd/config.toml
$ sudo systemctl restart containerd
$ sudo systemctl restart kubelet


### Initialize Kubernetes Cluster ###
$ sudo kubeadm init --pod-network-cidr=192.168.0.0/16 --control-plane-endpoint 13.38.113.90:6443

### Setup kubectl for regular user on master node ###
$ mkdir -p $HOME/.kube
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config

### Or as root user run ###
$ export KUBECONFIG=/etc/kubernetes/admin.conf

### Join worker nodes (over public-ip. If private-ip remove --control-plane-endpoint parameter) ###
<strong>$ sudo kubeadm join &#x3C;public-ip>:&#x3C;port> --token &#x3C;token> --discovery-token-ca-cert-hash &#x3C;hash>
</strong>
### Test if kubectl works and you can see all nodes. If node status is NotReady -> Install Calico ###
$ kubectl get nodes


### Install Pod network addon Calico ###
$ kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.1/manifests/tigera-operator.yaml
$ curl https://raw.githubusercontent.com/projectcalico/calico/v3.26.0/manifests/custom-resources.yaml -O
$ kubectl create -f custom-resources.yaml
</code></pre>

### Configuration

```
### Enable kubectl autocompletion ###
$ source <(kubectl completion bash)
$ echo "source <(kubectl completion bash)" >> ~/.bashrc
```
