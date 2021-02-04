# find cache path

1. search roles for ansible

2.  open kubespray/roles/download/defaults/main.yml

find "kubelet_download_url", and search

get follow result:

~~~yaml
  kubelet:
    enabled: true
    file: true
    version: "{{ kube_version }}"
    dest: "{{ local_release_dir }}/kubelet-{{ kube_version }}-{{ image_arch }}"
    sha256: "{{ kubelet_binary_checksum }}"
    url: "{{ kubelet_download_url }}"
    unarchive: false
    owner: "root"
    mode: "0755"
    groups:
    - k8s-cluster
~~~

search "local_release_dir", get:

local_release_dir: /tmp/releases

# find default version

1. open 