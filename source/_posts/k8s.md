---
title: K8S搭建
date: 2019-06-04 02:01:01
tags: k8s
categories: basic
---
### 安装
```bash
sudo swapoff -a

sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    stable"

 sudo apt install -y docker.io

sudo systemctl start docker

sudo systemctl enable docker

sudo apt-get update && sudo apt-get install -y apt-transport-https && curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

 echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list && sudo apt-get update

 sudo apt install -y kubeadm  kubelet kubernetes-cni
 systemctl enable kubelet && systemctl start kubelet

# master
 kubeadm init --kubernetes-version=1.15.0

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

	
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
## 或者
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
# worker
kubeadm join 192.168.1.18:6443 --token sexemg.1wqeqweqweqwew \
    --discovery-token-ca-cert-hash sha256:2153e47c21dbbaasdasd64735d4aa0546fdb60231 


kubeadm reset

```