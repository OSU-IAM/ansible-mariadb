# ansible-mariadb

An ansible playbook for MariaDB on CentOS 7

## Roles

* `database` - a standalone MariaDB server

## Pre-requisites

This playbook assumes that there is a separate disk device available for database data. For an example virtual disk configuration, see the [Terraform MariaDB module](https://github.sig.oregonstate.edu/IAM/terraform-modules/tree/master/mariadb). **It is assumed that the data disk device is `/dev/sdb`.**

## Configuration

For `dev`, `stage`, `prod`, etc., create an inventory file for each infrastructure type.

Example `inventory/dev`:

```
[dev]
mariadb-dev.someplace.edu
```

Then, create a variable file for each `dev`, `stage`, `prod` type in the inventory.

Example `group_vars/dev`:

```
---
mariadb:
  datadir: /data/mysql
  innodb_buffer_pool_size: changeme
  max_allowed_packet: changeme
```

## Running the Playbook

```
$ ansible-playbook -i inventory/dev site.yml
```
