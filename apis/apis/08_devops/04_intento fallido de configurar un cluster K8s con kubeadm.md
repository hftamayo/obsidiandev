==primer intento:

![[Pasted image 20240126152431.png]]


==segundo intento de levantar el cluster de k8s:

leyendo el journal que pipee con: 

sudo journalctl -u kubelet -n 25 > kubelet.journal

![[Pasted image 20240131120007.png]]


ejecutando estos comandos:

==sudo kubeadm reset
sudo kubeadm init --pod-network-cidr=10.244.0.0/16


Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 10.2.0.238:6443 --token jsmqn1.mutrzter8e38tyrv \
        --discovery-token-ca-cert-hash sha256:006ffc47e6789e1f37da97975592a386cfa0b33a41489468b3a2ccd9c867fcc6

haciendo un ==sudo systemctl status kubelet encontré que tampoco se podía conectar asi que quite docker y kubernetes y los volvi a instalar:


241  sudo systemctl stop kubelet
  242  sudo systemctl stop docker
  243  sudo apt-get purge docker-ce docker-ce-cli containerd.io
  244  sudo apt-get purge kubeadm kubectl kubelet kubernetes-cni kube*
  245  sudo apt-get purge kubeadm kubectl kubelet kubernetes-cni
  246  sudo rm -rf /var/lib/docker
  247  sudo rm -rf ~/.kube
  248  clear
  249  sudo apt-get update
  250  clear
  251  sudo apt-get install docker-ce docker-ce-cli containerd.io
  252  sudo apt-get install -y kubelet kubeadm kubectl


verificando status:

==docker esta arriba pero verificando kubelet aparece en exit, inicio el servicio y luego verificando status sale en exit, verificando el journal:


sudo journalctl -u kubelet  -n 25  > kubelet.journal encontré que hace referencia a un yaml, probabablemente de mi instalación previa


aca ya caí en un rabit hole por tanto dejo este enfoque, seguiré basándome en este tutorial: https://www.linuxtechi.com/install-kubernetes-cluster-on-debian/


