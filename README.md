# Ansible role: labocbz.install_logstash

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Crombez-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: SSL/TLS](https://img.shields.io/badge/Tech-SSL%2FTLS-orange)
![Tag: Logstash](https://img.shields.io/badge/Tech-Logstash-orange)
![Tag: Cluster](https://img.shields.io/badge/Tech-Cluster-orange)

An Ansible role install and configure Logstash your server.

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
install_logstash_major_version: "8"

install_logstash_config_path: "/etc/logstash"
install_logstash_ssl_path: "{{ install_logstash_config_path }}/ssl"
install_logstash_pipelines_path: "{{ install_logstash_config_path }}/pipelines"
install_logstash_pipelines_reload: "5s"
install_logstash_pipelines:
  - name: "main"
    workers: 1
    beats:
      - 5045
  - name: "main2"
    workers: 1
    beats:
      - 5046
  - name: "main3"
    workers: 1
    beats:
      - 5047

install_logstash_health_check_pipeline:
  name: "health"
  workers: 1
  beats: 
    - 5043

install_logstash_distributor_pipeline:
  name: "distributor"
  workers: 5
  beats: 
    - 5044

install_logstash_beats_ssl: false
install_logstash_api: true
install_logstash_api_ports: "9600-9700"
install_logstash_api_auth_type: "basic"
install_logstash_api_auth_login: "admin"
install_logstash_api_auth_password: "s3cUreP4$$w0rD"
install_logstash_api_ssl: false
install_logstash_api_jks: "/etc/logstash/ssl/my.jks.jks"
install_logstash_api_jks_password: "secretPassword"
install_logstash_host: "0.0.0.0"

install_logstash_cluster_name: "my.logstash-cluster.tld"

install_logstash_data_path: "/var/lib/logstash/data"

install_logstash_group: "logstash"
install_logstash_user: "logstash"
install_logstash_ram: "1g"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---

inv_install_logstash_major_version: 8
inv_install_logstash_config_path: "/etc/logstash"
inv_install_logstash_ssl_path: "{{ inv_install_logstash_config_path }}/ssl"
inv_install_logstash_pipelines_path: "{{ inv_install_logstash_config_path }}/pipelines"
inv_install_logstash_pipelines_reload: "5s"
inv_install_logstash_pipelines:
  - name: "main"
    workers: 1
    beats:
      - 5045
  - name: "main2"
    workers: 1
    beats:
      - 5046
  - name: "main3"
    workers: 1
    beats:
      - 5047

inv_install_logstash_health_check_pipeline:
  name: "health"
  workers: 1
  beats: 
    - 5043

inv_install_logstash_distributor_pipeline:
  name: "distributor"
  workers: 5
  beats: 
    - 5044

inv_install_logstash_beats_ssl: true

inv_install_logstash_api: true
inv_install_logstash_api_ports: "9600"
inv_install_logstash_api_auth_type: "basic"
inv_install_logstash_api_auth_login: "admin"
inv_install_logstash_api_auth_password: "s3cUreP4$$w0rD"
inv_install_logstash_api_ssl: true
inv_install_logstash_api_jks: "{{ inv_install_logstash_ssl_path }}/{{ inv_install_logstash_cluster_name }}/{{ inv_install_logstash_cluster_name }}.jks"
inv_install_logstash_api_jks_password: "secret"
inv_install_logstash_host: "0.0.0.0"

inv_install_logstash_cluster_name: "my.logstash-cluster.tld"

inv_install_logstash_data_path: "/var/lib/logstash/data"

inv_install_logstash_group: "logstash"
inv_install_logstash_user: "logstash"
inv_install_logstash_ram: "1g"

```

```YAML
# From AWX / Tower
---
all vars from to put/from AWX / Tower
```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_logstash"
  tags:
    - "labocbz.install_logstash"
  vars:
    install_logstash_major_version: "{{ inv_install_logstash_major_version }}"
    install_logstash_config_path: "{{ inv_install_logstash_config_path }}"
    install_logstash_ssl_path: "{{ inv_install_logstash_ssl_path }}"
    install_logstash_pipelines_path: "{{ inv_install_logstash_pipelines_path }}"
    install_logstash_pipelines_reload: "{{ inv_install_logstash_pipelines_reload }}"
    install_logstash_pipelines: "{{ inv_install_logstash_pipelines }}"
    install_logstash_health_check_pipeline: "{{ inv_install_logstash_health_check_pipeline }}"
    install_logstash_api: "{{ inv_install_logstash_api }}"
    install_logstash_api_ports: "{{ inv_install_logstash_api_ports }}"
    install_logstash_api_auth_type: "{{ inv_install_logstash_api_auth_type }}"
    install_logstash_api_auth_login: "{{ inv_install_logstash_api_auth_login }}"
    install_logstash_api_auth_password: "{{ inv_install_logstash_api_auth_password }}"
    install_logstash_host: "{{ inv_install_logstash_host }}"
    install_logstash_cluster_name: "{{ inv_install_logstash_cluster_name }}"
    install_logstash_data_path: "{{ inv_install_logstash_data_path }}"
    install_logstash_group: "{{ inv_install_logstash_group }}"
    install_logstash_user: "{{ inv_install_logstash_user }}"
    install_logstash_ram: "{{ inv_install_logstash_ram }}"
    install_logstash_api_ssl: "{{ inv_install_logstash_api_ssl }}"
    install_logstash_api_jks: "{{ inv_install_logstash_api_jks }}"
    install_logstash_api_jks_password: "{{ inv_install_logstash_api_jks_password }}"
    install_logstash_beats_ssl: "{{ inv_install_logstash_beats_ssl }}"
    install_logstash_distributor_pipeline: "{{ inv_install_logstash_distributor_pipeline }}
  ansible.builtin.include_role:
    name: "labocbz.install_logstash"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-05-22: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2023-05-30: Cryptographic update

* SSL/TLS Materials are not handled by the role
* Certs/CA have to be installed previously/after this role use

### 2023-06-05: Distributor pattern

* Implement a distributor pattern to redirect flux to their pipeline in case of Filebeat have to send multiple type of pipelines logs/metrics

### 2023-08-01: Cert and Key update

* You can now use custom name with the role for key and cert
* JKS is now created by the role
* P12 and JKS have the same password

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
