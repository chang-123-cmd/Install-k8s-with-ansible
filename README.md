# one-button install k8s
###### author qingxuanzi

* <a href="#demo1"> project describe function </a>
* <a href="#demo2"> Instructions </a>

## project describe function
### 1.prepare work
   * ntp
   * chrony
   * disable selinux
   * set firewalld
   * set swap
   * set kernel
### 2.Install yum repo
   * install httpd
   * unzip rpm package
### 3.Install docker
   * set config
   * install docker
### 4.Install docker registry
   * unzip package
   * run registry service
### 5.Install k8s
   * Install kubeadm/kubelet/kubectl
   * master init
   * add node
   * appply ingress 
   * appply dashboard
### 6.Install ceph
   * install ceph




## Instructions
### 1.import  package for yum repo and docker registry
Baidu cloud address:
>leave a message in issue

Put the following files in this directory in the project： \
{{ projectname }}/artifacts/  
>containerd-selinux-v2.119.2.tar \
>docker-ce-stable-v20.10.7.tar \
>kubernetes-v1.21.2.tar \
>registry-1.21.2.tar.gz 

baiduyunpan：
链接：https://pan.baidu.com/s/1XF-W3R-E6pWK2st-k1r3Qg
提取码：jsth

### 2.Write hosts.ini
```lombok.config
[all:vars]
ansible_connection=ssh
ansible_user=zhangsan
ansible_password=9876557
# same as `ansible_ssh_pass`
ansible_become_pass=9876557

[kubernetes:children]
kubernetes_master
kubernetes_master_slaves
kubernetes_slaves

[kubernetes_master_all:children]
kubernetes_master
kubernetes_master_slaves

[kubernetes_master]
172.16.**.***

[kubernetes_master_slaves]

[kubernetes_slaves]
172.16.**.***
172.16.**.***

[docker:children]
kubernetes
registry

[registry]
172.16.**.***

[repo]
172.16.**.***



```

### 3.Write shared.yml
```yaml
#---------------------------------
## Docker image  repo
##---------------------------------
docker_repo_ip: "172.20.**.***"
docker_repo_address: "***k8s-registry-address***:**port**"
docker_repo_domain_name: "***k8s-registry-address***"

master_ip: "172.20.**.***"
kubernetes_api: "apiserver.*****.local"   
#---------------------------------
###  package  repo
###---------------------------------
#
repo: "172.20.**.***"

```

### 4.Check the of each role defult/main.yml file
```bash
check eack role's defult/main.yml file ，set parameter
```

### 5. Install
>{{projectname}} is project name ,is k8s-ansible/
```bash
cd {{projectname}} 
ansible-playbook ./playbooks/01_prepare.yml
ansible-playbook ./playbooks/02_yum_repo.yml
ansible-playbook ./playbooks/03_docker_install.yml
ansible-playbook ./playbooks/04_install_docker_register.yml
ansible-playbook ./playbooks/05_install_k8s.yml
ansible-playbook ./playbooks/06_install_ceph.yml

```
f you want to test whether the configuration file is correct first, as in the above steps, add -C after the instruction, for example:
```bash
ansible-playbook ./playbooks/01_prepare.yml -C
```

### 6. issue
#### 1.When the running is interrupted, you want to repeat it.

#### 2.ceph安装用简单的块设备就可以识别


## 版本列表
kubeadm v1.21.2                         \
cni:v3.20.2                             \
pod2daemon-flexvol:v3.20.2              \
node:v3.20.2                            \
kube-controllers:v3.20.2                \
ingress-nginx-controller:0.30.0         \
metrics-scraper:v1.0.6                  \
dashboard:v2.2.0                        \
metrics-server:0.4.4                    \
                                        \
coredns:1.8.0                           \
etcd:3.4.13-0                           \
pause:3.4.1                             \
kube-proxy:v1.21.2                      \
kube-scheduler:v1.21.2                  \
kube-controller-manager:v1.21.2         \
kube-apiserver:v1.21.2                  \
                                        \
                                        \
ceph:v1.6.7                             \
cephcsi:v3.3.1                          \
csi-node-driver-registrar:v2.2.0        \
csi-resizer:v1.2.0                      \
csi-provisioner:v2.2.2                  \
csi-snapshotter:v4.1.1                  \
csi-attacher:v3.2.1                     \
ceph:v15.2.13                           

