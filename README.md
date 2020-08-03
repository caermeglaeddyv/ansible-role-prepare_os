Ansible role: Prepare OS
=========

Originally this role was used to do common preparations for all infrastructure hosts used in kubernetes project.

For now, it does the following tasks:
- replaces uppercase hostname to lowercase in hosts file
- sets system hostname to the name of host from inventory file
- adds every inventory host name to hosts file with default ipv4 address
- adds any extra entries to hosts file using group and host vars
- upgrades all system packages to the latest ones
- configures ansible user with passwordless sudo (it must be sudo already or root password must be provided via cli or using variables)
- installs chrony if missing, configures timezone and ntp
- deploys ssh public key from pre-created keypair to every machine and disables ssh password auth and root login


Requirements
------------

This is not strict requirements and it may not work with other versions than tested ones.
Anyway. feel yourself free to test by yourself, suggest addition of new functionality and contribute.

Role is tested with:
- Ansible version >= 2.8.6
- CentOS version >= 7.6 (1803)

Pre-created ssh keypair must be present in the defined path.


Role Variables
--------------

Variables and their descriptions copied from defaults/main.yml

```bash

# Array of extra /etc/hosts entries which need to be added to your host(s).
# You can use group_vars and host_vars functionality to separate entries for
# inventory groups and hosts respectively. Each array member is a string which
# repeats line in /etc/hosts file:
prepare_os_extra_etc_hosts: []
# - 1.2.3.4     abc.yxz

# Version of chrony package:
prepare_os_chrony_version: 3.4

# Timezone to set with timedatectl utility:
prepare_os_timezone: UTC

# Path to ssh public key which will be deployed to every inventory host
# (absolute or relative to directory from which playbook is executed),
# if defined, it will also disable password authentication on ssh server:
prepare_os_ssh_pubkey_file_path: 

```


Dependencies
------------

Pre-created ssh keypair must be present in the defined path.


Example Playbook
----------------

```bash
---
- hosts: localhost
  gather_facts: false
  become: no
  tasks:
  - name: Check ansible version >=2.8.6
    assert:
      msg: Ansible must be v2.8.6 or higher
      that:
      - ansible_version.string is version("2.8.6", ">=")
    tags:
    - check
  vars:
    ansible_connection: local

- hosts: all
  become: yes
  tasks:
  - import_role:
      name: prepare_os

```

More detailed examples ( inventories, playbooks etc. ) of this and other roles can be found [here](https://github.com/caermeglaeddyv/examples/tree/dev/ansible).

It's highly recommended to start you test deploys from there, especially if you use Google Cloud Platform or VMware vCenter as your infrastructure, for now that [repository](https://github.com/caermeglaeddyv/examples) contains [Packer](https://github.com/caermeglaeddyv/examples/tree/dev/packer) and [Terraform](https://github.com/caermeglaeddyv/examples/tree/dev/terraform) examples to build templates and deploy machines on this platforms.


License
-------

[Apache 2.0](https://github.com/caermeglaeddyv/ansible-role-rear/blob/dev/LICENSE)


Author Information
------------------

Copyright 2020 [caermeglaeddyv](https://github.com/caermeglaeddyv)
