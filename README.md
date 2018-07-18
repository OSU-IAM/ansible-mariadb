# ansible-mariadb

An ansible playbook for MariaDB on CentOS 7

## Roles

* `database` - a standalone MariaDB server

## Configuration

For `dev`, `stage`, `prod`, etc., create an inventory and variable file for each infrastructure type.

Example `inventory/dev`:

```
[dev]
mariadb-dev.someplace.edu
```

## Running the Playbook

```
$ ansible-playbook -i inventory/dev site.yml
```
