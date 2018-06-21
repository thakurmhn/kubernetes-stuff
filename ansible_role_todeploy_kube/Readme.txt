Prerequisites:
platform : centos7
Ansible: 2.5

Ansible role to setup 1 master +2 node kubernetes cluster (more nodes can be added)

setup centos VMs 
configure hostnames 
Update hosts file template in ../roles/kubernetes-deploy/files/hosts.template with host names and ipaddress
setup password less auth between your Ansible host and Kubernetes nodes

$ ssh-copyid root@kube-nodes?

# setup Ansible inventory 
# E.g

[kube]
kube-master.mhn.com hostrole=master
kube-node1.mhn.com  hostrole=node
kube-node2.mhn.com  hostrole=node


Run Ansible Role

$ ansible-playbook install-kubernetes-centos7.yml

Role does follwoing 
- updated os
- reboot
- setup kubernetes environment 

upon completion of ansible play,  copy following command from stdout of play and run on all node as root



kubeadm join 192.168.2.240:6443 --token ce2b82.hbu4u9x12luwbhyr --discovery-token-ca-cert-hash sha256:510573c7ec722ac20674e96403517e97696e2110635d57455d869bae06ffefaa

- Validation on Master

# kubectl get nodes  (check node status)
# kubectl get pods --all-namespaces  (you may need to wait for sometime to get the containers up)

# !!! You are Done !!! 
