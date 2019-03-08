[![Build Status](https://travis-ci.org/KrakenSystems/wg-cni.svg?branch=master)](https://travis-ci.org/KrakenSystems/wg-cni)

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
[all]
ip-172-31-33-212.eu-west-1.compute.internal ansible_host=34.242.32.179 ansible_ssh_user=ubuntu ip=10.100.0.1  wg_public_ip=34.242.32.179 pod_cidr=10.101.1.0/24
ip-172-31-46-31.eu-west-1.compute.internal  ansible_host=52.48.73.20 ansible_ssh_user=ubuntu   ip=10.100.0.2  wg_public_ip=52.48.73.20   pod_cidr=10.101.2.0/24
ip-172-31-46-55.eu-west-1.compute.internal  ansible_host=63.34.8.93 ansible_ssh_user=ubuntu    ip=10.100.0.3  wg_public_ip=63.34.8.93    pod_cidr=10.101.3.0/24

[kube-master]
ip-172-31-33-212.eu-west-1.compute.internal
ip-172-31-46-31.eu-west-1.compute.internal
ip-172-31-46-55.eu-west-1.compute.internal


[kube-node]
ip-172-31-33-212.eu-west-1.compute.internal
ip-172-31-46-31.eu-west-1.compute.internal
ip-172-31-46-55.eu-west-1.compute.internal

[etcd]
ip-172-31-33-212.eu-west-1.compute.internal
ip-172-31-46-31.eu-west-1.compute.internal
ip-172-31-46-55.eu-west-1.compute.internal

[k8s-cluster:vars]
wg_masq_cidr=10.100.0.0/15

[k8s-cluster:children]
kube-node
kube-master

[wireguard:children]
k8s-cluster

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
