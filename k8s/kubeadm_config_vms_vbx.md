## Italling kubernate cluster in virtualbox ubuntu vms
1- enable adbter 2 in virtualbox and set it to vbx host only adapter
2- enable the adapter in vms
```
$ sudo ifconfig enp0s8 up
```
3- set ip adresses in vms
Master node 
```
$ sudo ifconfig enp0s8 192.168.0.1 netmask 255.255.255.05
```
slave node 
```
$ sudo ifconfig enp0s8 192.168.0.1 netmask 255.255.255.05
```
4- install docker
```
$ sudo apt install docker.io
```
```
$ sudo systemctl enable docker
```
5- Add the Kubernetes signing key on both the nodes
```
$ sudo apt install curl
$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
```
6- Add Xenial Kubernetes Repository on both the nodes
```
$ sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
```
7- Install Kubeadm
```
$ sudo apt install kubeadm
```
8- Disable swap memory (if running) on both the nodes
You need to disable swap memory on both the nodes as Kubernetes does not perform properly on a system that is using swap memory. Run the following command on both the nodes in order to disable swap memory
```
$ sudo swapoff -a
```
9- Give Unique hostnames to each node
```
$ sudo hostnamectl set-hostname master-node
```
```
$ sudo hostnamectl set-hostname slave-node
```
10- initialize kubernate on the master node 
```
$ sudo kubeadm init --pod-network-cidr=192.168.0.1/16 --apiserver-advertise-address 192.168.0.2
```
To start using your cluster, you need to run the following as a regular user:
```
$ mkdir -p $HOME/.kube
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
$ sudo kubeadm join 192.168.0.2:6443 --token 43rd8k.68cgj31jlhc89cwl --discovery-token-ca-cert-hash sha256:366d90cc7fb5cb67e69c70cb8d4f796e67305d08bb55442eefd5a9962ad15a16
```
11- Deploy a Pod Network through the master node
```
$ sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
$ kubectl get pods --all-namespaces
$ sudo kubectl get nodes
```

source : https://vitux.com/install-and-deploy-kubernetes-on-ubuntu/