Manage Rancher Cluster
=========

This Role creates/deletes a k8s cluster in a Rancher environment by using a RKE template that should be already created.

When the deletion is triggered, this role remove the cluster from Rancher console and clean all docker images and folders from the nodes.

Requirements
------------

The role requires that docker is already installed on target hosts.
It also requires a RKE template created on Rancher.
Currently it just creates the cluster, but in the future it will support additional actions and input parameters.


Supports the following Operating Systems:

- CentOS 7

But should run fine in debian based linux.

Role Variables
--------------
Available variables are listed below:

Rancher URL API
```
rancher_host: console.myrancher.com
```

The name of cluster to be managed
```
rancher_clustername: mycluster
```

The state of the cluster
```
rancher_cluster_state: present/absent
```

Your access key (username). Can be created via Rancher UI, on you user avatar -> API & Keys
```
rancher_user: myuser
```

Your secret key (password). Can be created via Rancher UI, on you user avatar -> API & Keys
```
rancher_password: mypasswd
```

The RKE cluster template created on Rancher. Can be accessed and modified on "Tools -> RKE Templates"
Use here the ID of the template, retrieved by the "View in API" option.
```
rancher_cluster_template_id: ctr-rfsx
```

A list of roles for each node that will be used during the execution of add node command.
This list can be described in inventory file.
```
rancher_cluster_roles='["controlplane","etcd","worker"]'
```

This role creates the following facts:
```
clusterid as string
added_nodes as list
kubeconfig as string
```


Example Playbook
----------------

This examples installs docker with the ericsysmin.docker.docker role.
```
- name: Create Rancher Cluster
  hosts: all
  gather_facts: no
  vars:
    rancher_host: console.rancher.domain.com
    rancher_clustername: mycluster
    rancher_cluster_state: present
    rancher_user: token-bla
    rancher_password: blablablablablablablalbla
    rancher_cluster_template_id: ctr-rfsx6    
  roles:
    - role: ericsysmin.docker.docker
      vars:
        docker_storage_driver: overlay2
        docker_edition: ce
        docker_ee_version: 19.3.11
    - role: alexismaior.ansible_role_manage_rancher_cluster
```

This examples deletes a Rancher k8s cluster and fully lean its nodes.
```
- name: Delete Rancher Cluster
  hosts: all
  gather_facts: no
  vars:
    rancher_host: console.rancher.domain.com
    rancher_clustername: mycluster
    rancher_cluster_state: absent
    rancher_user: token-bla
    rancher_password: blablablablablablablalbla
    rancher_cluster_template_id: ctr-rfsx6    
  roles:
    - role: alexismaior.ansible_role_manage_rancher_cluster
```

License
-------

MIT / BSD

Author Information
------------------

This role was created in 2020 by Alexis Sotto Maior.
