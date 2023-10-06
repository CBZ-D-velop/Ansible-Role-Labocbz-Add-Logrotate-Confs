# Ansible role: labocbz.add_logrotate_confs

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Cbz-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: Logrotate](https://img.shields.io/badge/Tech-Logrotate-orange)

An Ansible role to install and add a Logrotate configuration on your host.

The Ansible role manages and adds Logrotate configurations to a target system. It installs Logrotate and add configuration files for various services and applications. These configurations define the rotation, archiving, compression, and periodic deletion of system logs and other log files, optimizing disk space usage and simplifying log management.

The role employs a modular approach by specifying configurations through a list of service names. Each service name corresponds to a specific logrotate configuration that is added or updated on the system.

With this flexible approach, extending the role is straightforward by adding new services to the list, making it easy to manage logrotate configurations for additional services without directly modifying the role's code.

Using this list of configurations centralizes and organizes logrotate configuration management. System administrators can easily add or remove items from the list to adjust logrotate configurations according to specific needs in their environment.

In summary, the role is a powerful tool for efficiently managing logrotate configurations on target systems. With its flexible and centralized approach, it provides a comprehensive solution for handling system log rotation, contributing to the smooth operation and maintenance of systems.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
logrotate_configurations: []
#  - "alternatives"
#  - "apache2"
#  - "apt"
#  - "bootlog"
#  - "btmp"
#  - "ceph-common"
#  - "certbot"
#  - "chrony"
#  - "confconsole"
#  - "corosync"
#  - "cron-apt"
#  - "dbconfig-common"
#  - "dpkg"
#  - "fail2ban"
#  - "glusterfs-common"
#  - "lighttpd"
#  - "modsecurity"
#  - "mysql-server"
#  - "nginx"
#  - "openvswitch-switch"
#  - "pve"
#  - "pve-firewall"
#  - "redis-server"
#  - "rkhunter"
#  - "rsyslog"
#  - "tklbam"
#  - "tor"
#  - "ubuntu-advantage-tools"
#  - "ufw"
#  - "unattended-upgrades"
#  - "wtmp"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_logrotate_local_configurations:
  - "unattended-upgrades"

inv_logrotate_configurations: >
  {{ inv_logrotate_local_configurations | default(['']) +
  inv_logrotate_group1_configurations | default(['']) +
  inv_logrotate_group2_configurations | default(['']) }}

inv_logrotate_group1_configurations:
  - "alternatives"
  - "apache2"
  - "apt"
  - "bootlog"
  - "btmp"
  - "ceph-common"
  - "certbot"
  - "chrony"
  - "confconsole"
  - "corosync"
  - "cron-apt"
  - "dbconfig-common"
  - "dpkg"
  - "fail2ban"
  - "glusterfs-common"
  - "lighttpd"
  - "modsecurity"
  - "mysql-server"
  - "nginx"
  - "openvswitch-switch"
  - "pve"
  - "pve-firewall"
  - "redis-server"
  - "rkhunter"
  - "rsyslog"
  - "tklbam"
  - "tor"
  - "ubuntu-advantage-tools"
  - "ufw"
  - "unattended-upgrades"
  - "wtmp"

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.add_logrotate_confs"
    tags:
    - "labocbz.add_logrotate_confs"
    vars:
    logrotate_configurations: "{{ inv_logrotate_configurations }}"
    ansible.builtin.include_role:
    name: "labocbz.add_logrotate_confs"
```

Additionnaly you can use this playbook, this script, this file content and hosts to create role on your local machine.

```YAML
# ./bootstrap_role.yml
---
- name: "bootstrap a role"
  hosts: "localhost"
  tasks:

    - name: "Include labocbz.bootstrap_role"
      tags:
        - "labocbz.bootstrap_role"
      vars:
        bootstrap_role_user: "{{ input_bootstrap_role_user }}"
        bootstrap_role_base_path: "{{ input_bootstrap_role_base_path }}"
        bootstrap_role_meta_author: "{{ input_bootstrap_role_meta_author }}"
        bootstrap_role_meta_namespace: "{{ input_bootstrap_role_meta_namespace }}"
        bootstrap_role_meta_role_name: "{{ input_bootstrap_role_meta_role_name }}"
        bootstrap_role_meta_description: "{{ input_bootstrap_role_meta_description }}"
        bootstrap_role_technologies: "{{ input_bootstrap_role_technologies }}"
      ansible.builtin.include_role:
        name: "roles/labocbz.bootstrap_role"

```

```YAML
#./inputs.yml
---
input_bootstrap_role_user: "lordrobincbz"
input_bootstrap_role_base_path: "../../roles" #from the playbook location

input_bootstrap_role_meta_author: "Lord Robin Crombez"
input_bootstrap_role_meta_namespace: "labocbz"
input_bootstrap_role_meta_role_name: "mytest"
input_bootstrap_role_meta_description: "An Ansible role to bootstrap and create other roles."

input_bootstrap_role_technologies:
  - "Ansible"
  - "Bootstrap"
  - "Boilerplate"
  - "Markdown"

```

```Shell
# ./bootstrap_role

#!/bin/bash

cd ../..

ansible-playbook -i ./tools/bootstrap_hosts/hosts -e "@./tools/bootstrap_role/input.yml" ./tools/bootstrap_role/bootstrap_role.yml

```

```Ini
# ./hosts
localhost ansible_connection=local
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-04-27: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
