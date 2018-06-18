Prerequisites:
platform : centos7
Ansible role to setup 1 master +2 node kubernetes cluster (more nodes can be added)

setup centos VMs 
configure hostnames
Add hosts entries in /etc/hosts on all kunernetes node and your ansible hosts
setup password less auth between your Ansible host and Kubernetes nodes

$ ssh-copyid root@kube-nodes?

Run Ansible Role

$ ansible-playbook install-kubernetes-centos7.yml

Role does follwoing 
- updated os
- reboot
- setup kubernetes environment 

upon completion of ansible play, run following command on Master node

# kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address <master-node-ip> --apiserver-cert-extra-sans <master-node-fqdn> 
 
Login as normlal user/ root and run floowing commands
 
# mkdir -p $HOME/.kube
# sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
# sudo chown $(id -u):$(id -g) $HOME/.kube/config

- Configure Fannel using below command :

#  kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

- Copy the 'kubeadm join' command from kube init output and run on all nodes

e.g

kubeadm join 192.168.2.240:6443 --token ce2b82.hbu4u9x12luwbhyr --discovery-token-ca-cert-hash sha256:510573c7ec722ac20674e96403517e97696e2110635d57455d869bae06ffefaa

- Validation on Master

# kubectl get nodes  (check node status)
# kubectl get pods --all-namespaces  (you may need to wait for sometime to get the containers up)

 
