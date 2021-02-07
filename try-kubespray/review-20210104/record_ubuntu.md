# cluster setting

* master: ubuntu, 192.168.186.140
* node: centos, 192.168.186.143

# SSH without passwd

~~~shell script
ssh-copy-id -i ~/.ssh/id_rsa.pub yong@192.168.186.143
ssh yong@192.168.186.143
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

# config cluster

~~~shell script
cp -rfp inventory/sample inventory/mycluster20210207001
vim inventory/mycluster20210207001/inventory.ini
~~~

modify like follows:

~~~
[all]
node1 ansible_host=192.168.186.140  ip=192.168.186.140 etcd_member_name=etcd1 ansible_ssh_user='yong'
node2 ansible_host=192.168.186.143  ip=192.168.186.143 ansible_ssh_user='yong' ansible_sudo_pass='111111'

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
https://192.168.186.140:31741/
* get token
~~~shell script
sudo kubectl -n kube-system describe $(sudo kubectl -n kube-system get secret -n kube-system -o name | grep namespace) | grep token
~~~

* or use node2 for visit
https://192.168.186.143:31741/

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

