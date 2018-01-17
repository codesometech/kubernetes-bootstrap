# kubernetes-bootstrap

The vagrant file will enable you to bootstrap master and worker nodes with needed libraries to configure kubernetes cluster.
After running the vagrant file, login to master

* vagrant ssh master01
* Run 'sudo kubeadm init  --pod-network-cidr=192.168.0.0/16 --apiserver-advertise-address=192.168.135.11'
