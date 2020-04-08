# ansible-kubernetes-stolon

Ansible role and sample playbook to deploy sorintlab/stolon on a Kubernetes cluster.

> Based on [sorintlab/stolon/examples/kubernetes](https://github.com/sorintlab/stolon/tree/master/examples/kubernetes).

## Prerequisites
1. Python3 and pip3
2. Mitogen is higly recommended:
  - download: `mkdir -p plugins && cd plugins && git clone https://github.com/dw/mitogen.git`
  - set in **sample-absible.cfg**:
    - `strategy_plugins = plugins/mitogen/ansible_mitogen/plugins/strategy`
    - `strategy = mitogen_linear`
3. Setup ansible inventory:
    - To run remotely:
        - Set your ansible_host in `inventory/host_vars/master` to point to your kubernetes master
        - Set up your ssh key for authentication to the master
    - To run run locally:
        - in `inventory/host_vars/master`, uncomment: `ansible_connection: local`
        - set the playbook variable `kubeconfig_file_path` pointing to a local kueconfig

## Run
```bash
ANSIBLE_SSH_PIPELINING=true ANSIBLE_CONFIG=sample-ansible.cfg ansible-playbook sample-playbook.yml
```

## Role parameters

| Parameter                           | Default Value                | Description                                                                                                                     |
|-------------------------------------|------------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| stolon_namespace                    | stolon                       | Kubernetes namespace                                                                                                            |
| stolon_release                      | ''                           | Optional prefix for stolon objetcs                                                                                              |
| stolon_rbac                         | true                         | Enable RBAC                                                                                                                     |
| stolon_cluster                      | kube-stolon                  | Stolon cluster name. Will be prefixed with `stolon_release`                                                                     |
| stolon_keeper_replicas              | 2                            | Number of keeper (statefulset) replicas                                                                                         |
| stolon_proxy_replicas               | 2                            | Number of proxy (deployment) replicas                                                                                           |
| stolon_sentinel_replicas            | 2                            | Number of sentinel (deployment) replicas                                                                                        |
| stolon_image                        | sorintlab/stolon:master-pg12 | Docker image for all stolon components                                                                                          |
| stolon_secret_password              | `undefined`                  | Password for `stolon` user. If not specified, a 15 length random password will be generated. Must NOT be base64 encoded.        |
| stolon_secret_replpassword          | `undefined`                  | Password for the replication user. If not specified, a 15 length random password will be generated. Must NOT be base64 encoded. |
| |
| stolon_stkeeper_debug               | false                        | Enable debug for keeper                                                                                                         |
| stolon_stproxy_debug                | false                        | Enable debug for proxy                                                                                                          |
| stolon_stsentinel_debug             | false                        | Enable debug for sentinel                                                                                                       |
| |
| stolon_storage_class                | stolon-local-storage         | Name of k8s storage class to be created/used                                                                                    |
| stolon_storage_class_reclaim_policy | Retain                       | Reclaim policy, overriding k8s default of `Delete`                                                                              |
| |
| stolon_storage_size                 | 1Gi                          | Size of the local PersistentVolume                                                                                              |
| stolon_storage_local_path           | /stolon-local-data           | Data directory. PostgreSQL data will be in `stolon_storage_local_path`/postgres                                                 |
| |
| stolon_proxy_service.externalIPs    | `undefined`                  | Array of IPs for exposing proxy-service                                                                                         |
| stolon_proxy_service.port           | 5432                         | External proxy port                                                                                                             |
| |
| stolonctl_retries                   | 5                            | Number of retries for the stolonctl task before giving up                                                                       |
| stolonctl_delay                     | 15                           | Delay between each retry                                                                                                        |
