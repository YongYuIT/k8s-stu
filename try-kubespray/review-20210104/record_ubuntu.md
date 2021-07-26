# cluster setting

* master: ubuntu, 192.168.186.140
* node: centos, 192.168.186.143

change hosts config for two servers

~~~shell script
sudo vi /etc/hosts
~~~

~~~
192.168.186.140	master
192.168.186.143	node
~~~


# SSH without passwd

~~~shell script
ssh-copy-id -i ~/.ssh/id_rsa.pub yong@node
ssh yong@node
~~~

# install kubespray

~~~shell script
$ git clone https://github.com/kubernetes-sigs/kubespray
$ git show
commit 1a91792e7ce15610c9567a5a4f04ff0fdb4bc43b (HEAD -> master, origin/master, origin/HEAD)
Author: Geonju Kim <geonju42@gmail.com>
Date:   Sat Feb 6 02:28:53 2021 +0900

    Change the owner of `/etc/crictl.yaml` to `root` (#7254)

$ cd kubespray
$ sudo apt-get install aptitude
$ sudo aptitude install python-pip python3-pip python3
$ sudo pip3 install -r requirements.txt
~~~

if error: Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-McitVN/ruamel.yaml.clib/

~~~shell script
pip3 install --upgrade pip
pip3 install paramiko
~~~

if error: No matching distribution found for ansible==2.9.16 (from -r requirements.txt (line 1))

~~~shell script
pip3 install ansible==2.9.16 -i https://pypi.tuna.tsinghua.edu.cn/simple
~~~

# config cluster

~~~shell script
cp -rfp inventory/sample inventory/mycluster20210207001
vim inventory/mycluster20210207001/inventory.ini
~~~

modify like follows:

~~~
[all]
node1 ansible_host=master etcd_member_name=etcd1 ansible_ssh_user='yong'
node2 ansible_host=node ansible_ssh_user='yong' ansible_sudo_pass='111111'

[kube-master]
node1

[etcd]
node1

[kube-node]
node1
node2

[calico-rr]

[k8s-cluster:children]
kube-master
kube-node
calico-rr
~~~

~~~shell script
vim roles/kubespray-defaults/defaults/main.yaml
~~~
dashboard_enabled: false --> true

~~~shell script
vim roles/kubernetes-apps/ansible/templates/dashboard.yml.j2
~~~
add "type: NodePort" to kubernetes-dashboard service

# start build

~~~shell script
ansible-playbook -i inventory/mycluster20210207001/inventory.ini cluster.yml -b -v
~~~

~~~shell script
$ sudo kubectl get svc --all-namespaces
NAMESPACE     NAME                        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                  AGE
default       kubernetes                  ClusterIP   10.233.0.1      <none>        443/TCP                  2m29s
kube-system   coredns                     ClusterIP   10.233.0.3      <none>        53/UDP,53/TCP,9153/TCP   44s
kube-system   dashboard-metrics-scraper   ClusterIP   10.233.24.225   <none>        8000/TCP                 38s
kube-system   kubernetes-dashboard        NodePort    10.233.48.175   <none>        443:31741/TCP            38s
~~~

* visit dashboard
https://master:31741/
* get token
~~~shell script
sudo kubectl -n kube-system describe $(sudo kubectl -n kube-system get secret -n kube-system -o name | grep namespace) | grep token
~~~

* or use node2 for visit
https://node:31741/

success!



## if hasn't add "type: NodePort" to kubernetes-dashboard service

~~~shell script
$ sudo kubectl get svc --all-namespaces
NAMESPACE     NAME                        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                  AGE
default       kubernetes                  ClusterIP   10.233.0.1      <none>        443/TCP                  2m38s
kube-system   coredns                     ClusterIP   10.233.0.3      <none>        53/UDP,53/TCP,9153/TCP   41s
kube-system   dashboard-metrics-scraper   ClusterIP   10.233.35.9     <none>        8000/TCP                 34s
kube-system   kubernetes-dashboard        ClusterIP   10.233.57.234   <none>        443/TCP                  34s

$ ifconfig
tunl0: flags=193<UP,RUNNING,NOARP>  mtu 1440
        inet 10.233.90.0  netmask 255.255.255.255
        tunnel   txqueuelen 1000  (IPIP Tunnel)
        RX packets 107  bytes 9361 (9.3 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 161  bytes 16514 (16.5 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

~~~

* visit dashboard
https://10.233.57.234:443/

* get token
~~~shell script
$ sudo kubectl -n kube-system describe $(sudo kubectl -n kube-system get secret -n kube-system -o name | grep namespace) | grep token
~~~

## if offline and yum by default is unavailable

~~~shell script
vim inventory/mycluster20210207001/group_vars/k8s-cluster/offline.yml
~~~

and modify (ali yum for eg.):

~~~
...
yum_repo: "http://mirrors.aliyun.com"
...
docker_rh_repo_base_url: "{{ yum_repo }}/docker-ce/linux/centos/$releasever/$basearch/stable/"
docker_rh_repo_gpgkey: "{{ yum_repo }}/docker-ce/gpg"
...
~~~

