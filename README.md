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
![Tag: Cluster](https://img.shields.io/badge/Tech-Server-orange)

An Ansible role install and configure Logstash your server.

The Logstash installation role automates the deployment of Logstash, a powerful real-time data processing system. It offers modular configuration through pipelines, enabling flexible and efficient Logstash setup.

The role provides various configuration options to tailor the Logstash installation to specific environments. It supports SSL encryption to ensure secure data transmission. SSL can be enabled for both incoming connections (beats) and outgoing connections (Elasticsearch).

Client authentication is also supported to control access to Logstash, using basic authentication for API access. User credentials (login and password) can be specified to restrict access to authorized users.

Pipelines empower advanced data filtering and processing. Multiple pipelines can be defined to handle different data types, enrich them, aggregate them, and send them to Elasticsearch.

The role seamlessly integrates with Elasticsearch, allowing authentication information to connect to Elasticsearch instances. Elasticsearch shard count can be configured for better performance and resource utilization.

Additionally, the role manages specific users and groups for Logstash execution, ensuring privilege separation and improved security.

In summary, this Logstash installation role offers a comprehensive approach to deploy and configure Logstash, encompassing security, modularity, authentication, and Elasticsearch integration. With this role, setting up and managing a Logstash system tailored to real-time data processing needs becomes effortless.

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
install_logstash__major_version: "8"

install_logstash__config_path: "/etc/logstash"
install_logstash__ssl_path: "{{ install_logstash__config_path }}/ssl"
install_logstash__pipelines_path: "{{ install_logstash__config_path }}/pipelines"
install_logstash__pipelines_reload: "5s"
install_logstash__pipelines:
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

install_logstash__health_check_pipeline:
  name: "health"
  workers: 1
  beats: 
    - 5043

install_logstash__distributor_pipeline:
  name: "distributor"
  workers: 5
  beats: 
    - 5044

install_logstash__beats_ssl: false
install_logstash__api: true
install_logstash__api_ports: "9600-9700"
install_logstash__api_auth_type: "basic"
install_logstash__api_auth_login: "admin"
install_logstash__api_auth_password: "s3cUreP4$$w0rD"
install_logstash__api_ssl: false
install_logstash__api_jks: "/etc/logstash/ssl/my.jks.jks"
install_logstash__api_jks_password: "secretPassword"
install_logstash__api_p12: "/etc/logstash/ssl/my.p12.p12"
install_logstash__ssl_p8: "/etc/logstash/ssl/my.p8.p8"
install_logstash__host: "0.0.0.0"
install_logstash__port: 5044
install_logstash__loglevel: "info"

install_logstash__ssl_elastic: true
install_logstash__ssl_elastic_jks: "{{ install_logstash__ssl_path }}/{{ install_logstash__cluster_name }}/{{ install_logstash__cluster_name }}.jks"
install_logstash__ssl_elastic_jks_password: "My3DD4fndfjff"
install_logstash__ssl_elastic_key: "{{ install_logstash__ssl_path }}/{{ install_logstash__cluster_name }}/{{ install_logstash__cluster_name }}.key"
install_logstash__ssl_elastic_crt: "{{ install_logstash__ssl_path }}/{{ install_logstash__cluster_name }}/{{ install_logstash__cluster_name }}.pem.crt"
install_logstash__ssl_elastic_p12: "{{ install_logstash__ssl_path }}/{{ install_logstash__cluster_name }}/{{ install_logstash__cluster_name }}.p12"
install_logstash__ssl_elastic_p8: "{{ install_logstash__ssl_path }}/{{ install_logstash__cluster_name }}/{{ install_logstash__cluster_name }}.pkcs8.p8"

install_logstash__cluster_name: "my.logstash-server.tld"
install_logstash__client_auth: true
install_logstash__ssl_crt: "/etc/ssl/cert.crt"
install_logstash__ssl_key: "/etc/ssl/key.key"
install_logstash__ssl_authorities: "/etc/ssl/cacert.pem"
install_logstash__elasticsearch_client_auth: false

install_logstash__data_path: "/var/lib/logstash/data"

install_logstash__group: "logstash"
install_logstash__user: "logstash"
install_logstash__ram: "1g"

install_logstash__elasticsearch_shard: 1
install_logstash__elasticsearch_ssl: true
install_logstash__elasticsearch_login: "user"
install_logstash__elasticsearch_password: "password"
install_logstash__elasticsearch_hosts:
  - "http://localhost:9200"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_prepare_host__system_users:
  - login: "elasticsearch"
    group: "elasticsearch"
  - login: "logstash"
    group: "logstash"

inv_install_logstash__major_version: 8
inv_install_logstash__config_path: "/etc/logstash"
inv_install_logstash__ssl_path: "{{ inv_install_logstash__config_path }}/ssl"
inv_install_logstash__pipelines_path: "{{ inv_install_logstash__config_path }}/pipelines"
inv_install_logstash__pipelines_reload: "5s"

inv_install_logstash__pipelines:
  - name: "apache2"
    workers: 1
    beats:
      - 5045
  - name: "fail2ban"
    workers: 1
    beats:
      - 5046
  - name: "logstash"
    workers: 1
    beats:
      - 5047

inv_install_logstash__health_check_pipeline:
  name: "health"
  workers: 1
  beats: 
    - 5043

inv_install_logstash__distributor_pipeline:
  name: "distributor"
  workers: 5
  beats: 
    - 5044

inv_install_logstash__beats_ssl: true

inv_install_logstash__api: true
inv_install_logstash__api_ports: "9600"
inv_install_logstash__api_auth_type: "basic"
inv_install_logstash__api_auth_login: "admin"
inv_install_logstash__api_auth_password: "s3cUreP4$$w0rD"
inv_install_logstash__api_ssl: true
inv_install_logstash__api_jks: "{{ inv_install_logstash__ssl_path }}/{{ inv_install_logstash__cluster_name }}/{{ inv_install_logstash__cluster_name }}.jks"
inv_install_logstash__api_jks_password: "My3DD4fndfjff"
inv_install_logstash__api_p12: "{{ inv_install_logstash__ssl_path }}/{{ inv_install_logstash__cluster_name }}/{{ inv_install_logstash__cluster_name }}.p12"
inv_install_logstash__ssl_key: "{{ inv_install_logstash__ssl_path }}/{{ inv_install_logstash__cluster_name }}/{{ inv_install_logstash__cluster_name }}.pem.key"
inv_install_logstash__ssl_crt: "{{ inv_install_logstash__ssl_path }}/{{ inv_install_logstash__cluster_name }}/{{ inv_install_logstash__cluster_name }}.pem.crt"
inv_install_logstash__ssl_p8: "{{ inv_install_logstash__ssl_path }}/{{ inv_install_logstash__cluster_name }}/{{ inv_install_logstash__cluster_name }}.pkcs8.p8"

inv_install_logstash__ssl_elastic: true
inv_install_logstash__ssl_elastic_jks: "{{ inv_install_logstash__ssl_path }}/{{ inv_install_logstash__cluster_name }}/{{ inv_install_logstash__cluster_name }}.jks"
inv_install_logstash__ssl_elastic_jks_password: "My3DD4fndfjff"
inv_install_logstash__ssl_elastic_key: "{{ inv_install_logstash__ssl_path }}/{{ inv_install_logstash__cluster_name }}/{{ inv_install_logstash__cluster_name }}.key"
inv_install_logstash__ssl_elastic_crt: "{{ inv_install_logstash__ssl_path }}/{{ inv_install_logstash__cluster_name }}/{{ inv_install_logstash__cluster_name }}.pem.crt"
inv_install_logstash__ssl_elastic_p12: "{{ inv_install_logstash__ssl_path }}/{{ inv_install_logstash__cluster_name }}/{{ inv_install_logstash__cluster_name }}.p12"
inv_install_logstash__ssl_elastic_p8: "{{ inv_install_logstash__ssl_path }}/{{ inv_install_logstash__cluster_name }}/{{ inv_install_logstash__cluster_name }}.pkcs8.p8"

inv_install_logstash__host: "0.0.0.0"
inv_install_logstash__client_auth: true
inv_install_logstash__beat_ssl_crt: "{{ inv_install_logstash__ssl_path }}/{{ inv_install_logstash__cluster_name }}/{{ inv_install_logstash__cluster_name }}.pem.crt"
inv_install_logstash__beat_ssl_key: "{{ inv_install_logstash__ssl_path }}/{{ inv_install_logstash__cluster_name }}/{{ inv_install_logstash__cluster_name }}.pem.key"
inv_install_logstash__ssl_authorities: "{{ inv_install_logstash__ssl_path }}/{{ inv_install_logstash__cluster_name }}/ca-chain.pem.crt"

inv_install_logstash__cluster_name: "my-logstash-server.domain.tld"

inv_install_logstash__data_path: "/var/lib/logstash/data"

inv_install_logstash__group: "logstash"
inv_install_logstash__user: "logstash"
inv_install_logstash__ram: "1g"
inv_install_logstash__loglevel: "info"

inv_install_logstash__elasticsearch_shard: 1
inv_install_logstash__elasticsearch_ssl: true
inv_install_logstash__elasticsearch_client_auth: true
inv_install_logstash__elasticsearch_login: "elastic"
inv_install_logstash__elasticsearch_password: "myVeryStringP@ssword"
inv_install_logstash__elasticsearch_hosts:
  - "molecule-local-instance-1-install-logstash:9200"

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
    install_logstash__major_version: "{{ inv_install_logstash__major_version }}"
    install_logstash__config_path: "{{ inv_install_logstash__config_path }}"
    install_logstash__ssl_path: "{{ inv_install_logstash__ssl_path }}"
    install_logstash__pipelines_path: "{{ inv_install_logstash__pipelines_path }}"
    install_logstash__pipelines_reload: "{{ inv_install_logstash__pipelines_reload }}"
    install_logstash__pipelines: "{{ inv_install_logstash__pipelines }}"
    install_logstash__health_check_pipeline: "{{ inv_install_logstash__health_check_pipeline }}"
    install_logstash__api: "{{ inv_install_logstash__api }}"
    install_logstash__api_ports: "{{ inv_install_logstash__api_ports }}"
    install_logstash__api_auth_type: "{{ inv_install_logstash__api_auth_type }}"
    install_logstash__api_auth_login: "{{ inv_install_logstash__api_auth_login }}"
    install_logstash__api_auth_password: "{{ inv_install_logstash__api_auth_password }}"
    install_logstash__host: "{{ inv_install_logstash__host }}"
    install_logstash__cluster_name: "{{ inv_install_logstash__cluster_name }}"
    install_logstash__data_path: "{{ inv_install_logstash__data_path }}"
    install_logstash__group: "{{ inv_install_logstash__group }}"
    install_logstash__user: "{{ inv_install_logstash__user }}"
    install_logstash__ram: "{{ inv_install_logstash__ram }}"
    install_logstash__api_ssl: "{{ inv_install_logstash__api_ssl }}"
    install_logstash__api_jks: "{{ inv_install_logstash__api_jks }}"
    install_logstash__api_jks_password: "{{ inv_install_logstash__api_jks_password }}"
    install_logstash__beats_ssl: "{{ inv_install_logstash__beats_ssl }}"
    install_logstash__client_auth: "{{ inv_install_logstash__client_auth }}"
    install_logstash__ssl_authorities: "{{ inv_install_logstash__ssl_authorities }}"
    install_logstash__distributor_pipeline: "{{ inv_install_logstash__distributor_pipeline }}"
    install_logstash__ssl_crt: "{{ inv_install_logstash__ssl_crt }}"
    install_logstash__ssl_key: "{{ inv_install_logstash__ssl_key }}"
    install_logstash__api_p12: "{{ inv_install_logstash__api_p12 }}"
    install_logstash__ssl_p8: "{{ inv_install_logstash__ssl_p8 }}"
    install_logstash__loglevel: "{{ inv_install_logstash__loglevel }}"
    install_logstash__elasticsearch_shard: "{{ inv_install_logstash__elasticsearch_shard }}"
    install_logstash__elasticsearch_ssl: "{{ inv_install_logstash__elasticsearch_ssl }}"
    install_logstash__elasticsearch_login: "{{ inv_install_logstash__elasticsearch_login }}"
    install_logstash__elasticsearch_password: "{{ inv_install_logstash__elasticsearch_password }}"
    install_logstash__elasticsearch_hosts: "{{ inv_install_logstash__elasticsearch_hosts }}"
    install_logstash__elasticsearch_client_auth: "{{ inv_install_logstash__elasticsearch_client_auth }}"
    install_logstash__ssl_elastic: "{{ inv_install_logstash__ssl_elastic }}"
    install_logstash__ssl_elastic_jks: "{{ inv_install_logstash__ssl_elastic_jks }}"
    install_logstash__ssl_elastic_jks_password: "{{ inv_install_logstash__ssl_elastic_jks_password }}"
    install_logstash__ssl_elastic_key: "{{ inv_install_logstash__ssl_elastic_key }}"
    install_logstash__ssl_elastic_crt: "{{ inv_install_logstash__ssl_elastic_crt }}"
    install_logstash__ssl_elastic_p12: "{{ inv_install_logstash__ssl_elastic_p12 }}"
    install_logstash__ssl_elastic_p8: "{{ inv_install_logstash__ssl_elastic_p8 }}"
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

### 2023-08-03: P8, filter and pipelines

* P8 file is handle by the role for ES output client auth
* cacert ar now in the ssl or client auth instead of client auth
* Distributor and health are now as file instead of dir
* All files of pipelines have the same name and are distingued by their path /name/beat.conf etc.
* Filter are now importer from files/ in the role, their name have to be name_filter.conf syntax
* The role is designed to be on one shot, so all conf are deleted before anything

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

### 2023-12-15: System users

* Role can now use system users and address groups

### 2023-12-27: Template and fixes

* Index templates now fixed
* Role is working, tested in ELK integration

### 2024-02-24: Fix and CI

* Added support for new CI base
* Edit all vars with __

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
