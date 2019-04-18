[![Build Status](https://travis-ci.org/KrakenSystems/wg-cni.svg?branch=master)](https://travis-ci.org/KrakenSystems/wg-cni)
[![Ansible Galaxy](https://img.shields.io/badge/galaxy-nmiculinic.wg_cni-5bbdbf.svg)](https://galaxy.ansible.com/nmiculinic/wg_cni)


wg-cni
=========

Install wireguard and sets it up as CNI for kubernetes.

It has integration with [wg-operator](https://github.com/KrakenSystems/wg-operator) for generating it's custom resource manifest.

See `defaults/main.yml` for all variable and their meaning.

Requirements
------------

Installed CNI plugins on the target host if you're using it as CNI plugin for k8s

Role Variables
--------------

See defaults/main.yml for documented variables description

All hosts should be in `wireguard` group.

`wg_public_ip` is set for all the server nodes, that's how the server is marked as such

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
ub18-k8s-1 wg_public_ip=5.2.236.7   wg_private_ip=10.100.0.1 pod_cidr=10.101.1.0/24
ub18-k8s-2 wg_public_ip=5.2.92.232  wg_private_ip=10.100.0.2 pod_cidr=10.101.2.0/24
ub18-k8s-3 wg_public_ip=5.2.89.93   wg_private_ip=10.100.0.3 pod_cidr=10.101.3.0/24

[bbb]
ads0901    wg_private_ip=10.100.0.4 pod_cidr=10.101.4.0/24

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

[wireguard:children]
bbb
kube-master

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
