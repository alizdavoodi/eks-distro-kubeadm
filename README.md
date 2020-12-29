## Deploy eks-distro with kubeadm 

This is an Ansible script that you can use to deploy [eks-distro](https://github.com/aws/eks-distro) with kubeadm deployment technique. I'm using cri-o for container runtime.

The script is idempotent, so you can add new nodes to the ansible node group and rerun the play.


### Prerequisite
* Install ansible 
* To implement init cluster, you need minimum of two nodes(master1, node1).
* add nodes IP to the ansible host file

```bash
$ cat << EOF > ./k8s-hosts
[master]
192.168.122.157 # Change it to the master ip 
[nodes]
192.168.122.57 # Change it to the nodes ip
EOF
```


### Run the playbook

```bash
ansible-playbook -i k8s-hosts -b --user=ubuntu k8s-cluster-setup.yaml
```


### Add new nodes
You can just add a new node IP address to the k8s-hosts file
```bash
$ cat << EOF > ./k8s-hosts
[master]
192.168.122.157 # Change it to the master ip 
[nodes]
192.168.122.57 # Change it to the nodes ip
192.168.122.67 # Change it to the nodes ip
```
rerun the play
```bash
ansible-playbook -i k8s-hosts -b --user=ubuntu k8s-cluster-setup.yaml
```

### Images and binaries
Project using official images and binaries that AWS published. you can find those dependencies here https://distro.eks.amazonaws.com/releases/v1-18-eks-1/

### Limitation
* There isn't any implementation for upgrade the cluster
* There isn't any implementation for arm, but eks-distro published arm images/binaries, so you might be able to deploy it with some small tweaks and change the images/binaries versions in group var file.
* the script doesn't support high-available/multi-master deployment