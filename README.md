# ansible-kubernetes-stolon

Ansible role and sample playbook to deploy sorintlab/stolon on a Kubernetes cluster.

> Based on [sorintlab/stolon/examples/kubernetes](https://github.com/sorintlab/stolon/tree/master/examples/kubernetes).

## Prerequisites
1. Python3 and pip3
2. Mitogen is higly recommended:
  - download: `mkdir -p plugins/mitogen && git clone https://github.com/dw/mitogen.git`
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
`ANSIBLE_SSH_PIPELINING=true ANSIBLE_CONFIG=sample-ansible.cfg ansible-playbook sample-playbook.yml`
