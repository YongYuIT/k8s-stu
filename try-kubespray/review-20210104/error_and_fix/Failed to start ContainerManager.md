# 错误现象

~~~
TASK [kubernetes/control-plane : kubeadm | Initialize first master] ******************************************************************************************************************************************************
fatal: [node1]: FAILED! => {"attempts": 3, "changed": true, "cmd": ["timeout", "-k", "300s", "300s", "/usr/local/bin/kubeadm", "init", "--config=/etc/kubernetes/kubeadm-config.yaml", "--ignore-preflight-errors=all", "--skip-phases=addon/coredns", "--upload-certs"], "delta": "0:01:57.919847", "end": "2021-07-26 17:08:40.689120", "failed_when_result": true, "msg": "non-zero return code", "rc": 1, "start": "2021-07-26 17:06:42.769273", "stderr": "W0726 17:06:42.816712   80823 utils.go:69] The recommended value for \"clusterDNS\" in \"KubeletConfiguration\" is: [10.233.0.10]; the provided value is: [169.254.25.10]
	[WARNING FileAvailable--etc-kubernetes-manifests-kube-apiserver.yaml]: /etc/kubernetes/manifests/kube-apiserver.yaml already exists
	[WARNING FileAvailable--etc-kubernetes-manifests-kube-controller-manager.yaml]: /etc/kubernetes/manifests/kube-controller-manager.yaml already exists
	[WARNING FileAvailable--etc-kubernetes-manifests-kube-scheduler.yaml]: /etc/kubernetes/manifests/kube-scheduler.yaml already exists
error execution phase wait-control-plane: couldn't initialize a Kubernetes cluster
To see the stack trace of this error execute with --v=5 or higher", "stderr_lines": ["W0726 17:06:42.816712   80823 utils.go:69] The recommended value for \"clusterDNS\" in \"KubeletConfiguration\" is: [10.233.0.10]; the provided value is: [169.254.25.10]", "	[WARNING FileAvailable--etc-kubernetes-manifests-kube-apiserver.yaml]: /etc/kubernetes/manifests/kube-apiserver.yaml already exists", "	[WARNING FileAvailable--etc-kubernetes-manifests-kube-controller-manager.yaml]: /etc/kubernetes/manifests/kube-controller-manager.yaml already exists", "	[WARNING FileAvailable--etc-kubernetes-manifests-kube-scheduler.yaml]: /etc/kubernetes/manifests/kube-scheduler.yaml already exists", "error execution phase wait-control-plane: couldn't initialize a Kubernetes cluster", "To see the stack trace of this error execute with --v=5 or higher"], "stdout": "[init] Using Kubernetes version: v1.20.2
[preflight] Running pre-flight checks
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
[certs] Using certificateDir folder \"/etc/kubernetes/ssl\"
[certs] Using existing ca certificate authority
[certs] Using existing apiserver certificate and key on disk
[certs] Using existing apiserver-kubelet-client certificate and key on disk
[certs] Using existing front-proxy-ca certificate authority
[certs] Using existing front-proxy-client certificate and key on disk
[certs] External etcd mode: Skipping etcd/ca certificate authority generation
[certs] External etcd mode: Skipping etcd/server certificate generation
[certs] External etcd mode: Skipping etcd/peer certificate generation
[certs] External etcd mode: Skipping etcd/healthcheck-client certificate generation
[certs] External etcd mode: Skipping apiserver-etcd-client certificate generation
[certs] Using the existing \"sa\" key
[kubeconfig] Using kubeconfig folder \"/etc/kubernetes\"
[kubeconfig] Using existing kubeconfig file: \"/etc/kubernetes/admin.conf\"
[kubeconfig] Using existing kubeconfig file: \"/etc/kubernetes/kubelet.conf\"
[kubeconfig] Using existing kubeconfig file: \"/etc/kubernetes/controller-manager.conf\"
[kubeconfig] Using existing kubeconfig file: \"/etc/kubernetes/scheduler.conf\"
[kubelet-start] Writing kubelet environment file with flags to file \"/var/lib/kubelet/kubeadm-flags.env\"
[kubelet-start] Writing kubelet configuration to file \"/var/lib/kubelet/config.yaml\"
[kubelet-start] Starting the kubelet
[control-plane] Using manifest folder \"/etc/kubernetes/manifests\"
[control-plane] Creating static Pod manifest for \"kube-apiserver\"
[control-plane] Creating static Pod manifest for \"kube-controller-manager\"
[control-plane] Creating static Pod manifest for \"kube-scheduler\"
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory \"/etc/kubernetes/manifests\". This can take up to 5m0s
[kubelet-check] Initial timeout of 40s passed.
[kubelet-check] It seems like the kubelet isn't running or healthy.
[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get \"http://localhost:10248/healthz\": dial tcp 127.0.0.1:10248: connect: connection refused.
[kubelet-check] It seems like the kubelet isn't running or healthy.
[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get \"http://localhost:10248/healthz\": dial tcp 127.0.0.1:10248: connect: connection refused.
[kubelet-check] It seems like the kubelet isn't running or healthy.
[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get \"http://localhost:10248/healthz\": dial tcp 127.0.0.1:10248: connect: connection refused.
[kubelet-check] It seems like the kubelet isn't running or healthy.
[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get \"http://localhost:10248/healthz\": dial tcp 127.0.0.1:10248: connect: connection refused.
[kubelet-check] It seems like the kubelet isn't running or healthy.
[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get \"http://localhost:10248/healthz\": dial tcp 127.0.0.1:10248: connect: connection refused.

	Unfortunately, an error has occurred:
		timed out waiting for the condition

	This error is likely caused by:
		- The kubelet is not running
		- The kubelet is unhealthy due to a misconfiguration of the node in some way (required cgroups disabled)

	If you are on a systemd-powered system, you can try to troubleshoot the error with the following commands:
		- 'systemctl status kubelet'
		- 'journalctl -xeu kubelet'

	Additionally, a control plane component may have crashed or exited when started by the container runtime.
	To troubleshoot, list all containers using your preferred container runtimes CLI.

	Here is one example how you may list all Kubernetes containers running in docker:
		- 'docker ps -a | grep kube | grep -v pause'
		Once you have found the failing container, you can inspect its logs with:
		- 'docker logs CONTAINERID'", "stdout_lines": ["[init] Using Kubernetes version: v1.20.2", "[preflight] Running pre-flight checks", "[preflight] Pulling images required for setting up a Kubernetes cluster", "[preflight] This might take a minute or two, depending on the speed of your internet connection", "[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'", "[certs] Using certificateDir folder \"/etc/kubernetes/ssl\"", "[certs] Using existing ca certificate authority", "[certs] Using existing apiserver certificate and key on disk", "[certs] Using existing apiserver-kubelet-client certificate and key on disk", "[certs] Using existing front-proxy-ca certificate authority", "[certs] Using existing front-proxy-client certificate and key on disk", "[certs] External etcd mode: Skipping etcd/ca certificate authority generation", "[certs] External etcd mode: Skipping etcd/server certificate generation", "[certs] External etcd mode: Skipping etcd/peer certificate generation", "[certs] External etcd mode: Skipping etcd/healthcheck-client certificate generation", "[certs] External etcd mode: Skipping apiserver-etcd-client certificate generation", "[certs] Using the existing \"sa\" key", "[kubeconfig] Using kubeconfig folder \"/etc/kubernetes\"", "[kubeconfig] Using existing kubeconfig file: \"/etc/kubernetes/admin.conf\"", "[kubeconfig] Using existing kubeconfig file: \"/etc/kubernetes/kubelet.conf\"", "[kubeconfig] Using existing kubeconfig file: \"/etc/kubernetes/controller-manager.conf\"", "[kubeconfig] Using existing kubeconfig file: \"/etc/kubernetes/scheduler.conf\"", "[kubelet-start] Writing kubelet environment file with flags to file \"/var/lib/kubelet/kubeadm-flags.env\"", "[kubelet-start] Writing kubelet configuration to file \"/var/lib/kubelet/config.yaml\"", "[kubelet-start] Starting the kubelet", "[control-plane] Using manifest folder \"/etc/kubernetes/manifests\"", "[control-plane] Creating static Pod manifest for \"kube-apiserver\"", "[control-plane] Creating static Pod manifest for \"kube-controller-manager\"", "[control-plane] Creating static Pod manifest for \"kube-scheduler\"", "[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory \"/etc/kubernetes/manifests\". This can take up to 5m0s", "[kubelet-check] Initial timeout of 40s passed.", "[kubelet-check] It seems like the kubelet isn't running or healthy.", "[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get \"http://localhost:10248/healthz\": dial tcp 127.0.0.1:10248: connect: connection refused.", "[kubelet-check] It seems like the kubelet isn't running or healthy.", "[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get \"http://localhost:10248/healthz\": dial tcp 127.0.0.1:10248: connect: connection refused.", "[kubelet-check] It seems like the kubelet isn't running or healthy.", "[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get \"http://localhost:10248/healthz\": dial tcp 127.0.0.1:10248: connect: connection refused.", "[kubelet-check] It seems like the kubelet isn't running or healthy.", "[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get \"http://localhost:10248/healthz\": dial tcp 127.0.0.1:10248: connect: connection refused.", "[kubelet-check] It seems like the kubelet isn't running or healthy.", "[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get \"http://localhost:10248/healthz\": dial tcp 127.0.0.1:10248: connect: connection refused.", "", "	Unfortunately, an error has occurred:", "		timed out waiting for the condition", "", "	This error is likely caused by:", "		- The kubelet is not running", "		- The kubelet is unhealthy due to a misconfiguration of the node in some way (required cgroups disabled)", "", "	If you are on a systemd-powered system, you can try to troubleshoot the error with the following commands:", "		- 'systemctl status kubelet'", "		- 'journalctl -xeu kubelet'", "", "	Additionally, a control plane component may have crashed or exited when started by the container runtime.", "	To troubleshoot, list all containers using your preferred container runtimes CLI.", "", "	Here is one example how you may list all Kubernetes containers running in docker:", "		- 'docker ps -a | grep kube | grep -v pause'", "		Once you have found the failing container, you can inspect its logs with:", "		- 'docker logs CONTAINERID'"]}

NO MORE HOSTS LEFT *******************************************************************************************************************************************************************************************************

PLAY RECAP ***************************************************************************************************************************************************************************************************************
localhost                  : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
node1                      : ok=511  changed=73   unreachable=0    failed=1    skipped=616  rescued=0    ignored=0 
~~~

# 查因

~~~
sudo vim /var/log/message
~~~

~~~
Jul 26 17:30:30 localhost kubelet: E0726 17:30:30.706988  111540 node_container_manager_linux.go:57] Failed to create ["kubepods"] cgroup
Jul 26 17:30:30 localhost kubelet: F0726 17:30:30.707002  111540 kubelet.go:1350] Failed to start ContainerManager Cannot set property TasksAccounting, or unknown property.
~~~

# 解决方案


* 升级systemd组件

ref to: https://cloud.tencent.com/developer/article/1827714

~~~
systemctl --version

pip3 install meson ninja
yum install libcap-devel libmount-devel xz-devel gperf -y
export PATH="/usr/local/bin:$PATH"
source ~/.bash_profile

wget https://github.com/systemd/systemd/archive/v247.tar.gz
tar zxf v247.tar.gz
cd systemd-247/

meson build/ && ninja -C build
cd build/
ninja install

kill -TERM 1

systemctl --version
~~~


* 升级systemd过程中可能会需要升级mount，升级方法是：

1. yum list installed | grep mount
2. 找对应的rpm包：https://pkgs.org/search/?q=libmount
3. wget下载rpm二进制文件包，然后yum install rpm包文件