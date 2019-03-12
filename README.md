[![Build Status](https://travis-ci.org/KrakenSystems/wg-cni.svg?branch=master)](https://travis-ci.org/KrakenSystems/wg-cni)
[![Ansible Galaxy](https://img.shields.io/badge/galaxy-nmiculinic.wg_cni-5bbdbf.svg)](https://galaxy.ansible.com/nmiculinic/wg_cni)


wg-cni
=========

Install wireguard and sets it up as CNI for kubernetes.

Requirements
------------

Installed CNI plugins on the target host.

Role Variables
--------------

See defaults/main.yml for documented variables description

All hosts should be in `wireguard` group. If fact `wg_public_ip` is set the node shall also act as endpoint for all others.

Dependencies
------------

CNI plugins need to be installed on the target machine.
Tested with version 0.6.0 and bridge + loopback plugins

Example Playbook
----------------

Example inventory:
```
# kube_pods_subnet: 10.101.0.0/16
# wg_listen_port = 5555 by default

[all]
ub18-k8s-1 ansible_host=10.251.0.141 wg_public_ip=5.2.236.7 ansible_ssh_user=ubuntu ip=10.100.0.1 pod_cidr=10.101.1.0/24
ub18-k8s-2 ansible_host=10.252.0.188 wg_public_ip=5.2.92.232  ansible_ssh_user=ubuntu ip=10.100.0.2 pod_cidr=10.101.2.0/24
ub18-k8s-3 ansible_host=10.254.0.8   wg_public_ip=5.2.89.93 ansible_ssh_user=ubuntu ip=10.100.0.3 pod_cidr=10.101.3.0/24

[bbb]
ADS0901     ansible_host=192.168.1.89                            ansible_ssh_user=ubuntu                          ip=10.100.0.4 pod_cidr=10.101.4.0/24

[kube-master]
ub18-k8s-1
ub18-k8s-2
ub18-k8s-3

[etcd]
ub18-k8s-1
ub18-k8s-2
ub18-k8s-3

[kube-node]
ub18-k8s-1
ub18-k8s-2
ub18-k8s-3

[kube-node:children]
bbb

[k8s-cluster:children]
kube-node
kube-master

[wireguard-server:children]
kube-master

[wireguard-client:children]
bbb

[wireguard:children]
wireguard-server
wireguard-client

[python3:children]
k8s-cluster

[python3:vars]
ansible_python_interpreter=/usr/bin/python3
```

License
-------

MIT

Author Information
------------------

Feel free to reach out via Issues, PRs or email
