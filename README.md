# Odoo Test Case

Odoo Test Case deploys an environment using vagrant, ansible and docker with 2 nginx instances, 3 Odoo v12 instances, 2 postgres servers in master-slave configuration and 1 monitoring server with InfluxDB and Grafana.

## Usage

## Prerequisites

Ansible is used from the local machine and an ansible-galaxy role is required.\
You need:
- Ansible
- Vagrant

Clone the repo, install ansible role and run from root folder:

```
$ ansible-galaxy install geerlingguy.nfs
$ vagrant up
```

To access both services you must put the domain and ip entries of one of the nginx instances in the local hosts file.
Example:

```
172.16.1.21 odoo.dev grafana.dev
```

## Configurations

### General config

- Vagrant Box: ubuntu/focal64
- Docker Image: odoo:12

### Nginx

Nginx is deployed in 2 instances balancing the requests between the 3 instances of Odoo. In addition, 2 virtual host configurations are provided for Odoo (odoo.dev) and Grafana (grafana.dev).

### Odoo

Odoo instances are deployed on 3 servers using docker-compose. The first server is used to generate the initial database and to share by NFS the data volume and the addons directory with the installation of the session_db module.

### PostgreSQL

For postgresql 2 instances are deployed in master - slave configuration. Repmgr is used to configure replication.

### Monitoring

The monitoring server has Grafana and InfluxDB deployed. Telegraf is deployed in each instance. The datasources and 2 dashboards are automatically provisioned, a general one with the uptime of each server from where you can go to a detailed view of the instance using the link of the gadget.