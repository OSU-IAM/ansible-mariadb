# ansible-mariadb

An ansible playbook for MariaDB on CentOS 7

This playbook is idempotent and is safe to run against running systems.

## Roles

* `database` - a standalone MariaDB server
* `midpoint` - create a database for midPoint

## Pre-requisites

This playbook assumes that there is a separate disk device available for database data. For an example virtual disk configuration, see the [Terraform MariaDB module](https://github.sig.oregonstate.edu/IAM/terraform-modules/tree/master/mariadb).

**IMPORTANT! It is assumed that the data disk device is `/dev/sdb`.**

## Configuration

For `dev`, `stage`, `prod`, etc., create an inventory file for each infrastructure type.

Example `inventory/dev`:

```
[dev]
mariadb-dev.someplace.edu
mariadb-dev2.someplace.edu

[midpoint]
mariadb-dev2.someplace.edu
```

To apply the `midpoint` role to a host, create a `[midpoint]` group and add the host(s) to it.

Then, create a variable file for each `dev`, `stage`, `prod` type in the inventory.

Example `group_vars/dev`:

```
---
mariadb:
  datadir: /data/mysql
  innodb_buffer_pool_size: changeme
  max_allowed_packet: changeme
  backupsdir: /data/backups
```

* `backupsdir` - dir for database backup dumpfiles. **NOTE:** backups are configured on a per-role basis. The `midpoint` role is an example of how this can be done.

### Configuring a midPoint Database

Add the following to the variable file:

```
midpoint:
  midpoint_host: changeme
  db_name: changeme
  db_username: changeme
  db_password: changeme
  db_create_script_url: https://raw.githubusercontent.com/Evolveum/midpoint/v3.8/config/sql/_all/mysql-3.8-all-utf8mb4.sql
```

* `midpoint_host` (required) - hostname of midPoint application server that will connect to this database
* `db_name` (required) - name of database to create
* `db_username` (required) - database user's username
* `db_password` (required) - database user's password
* `db_create_script_url` - path to the database creation script for the target version of midPoint

See `group_vars/example-dist` for an example.

## Running the Playbook

```
$ ansible-playbook -i inventory/dev site.yml
```
