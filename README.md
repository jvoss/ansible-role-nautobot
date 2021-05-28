# Ansible Role: Nautobot

[![CI](https://github.com/jvoss/ansible-role-nautobot/actions/workflows/ci.yml/badge.svg?branch=master)](https://github.com/jvoss/ansible-role-nautobot/actions/workflows/ci.yml)

Installs and configures [Nautobot](https://github.com/nautobot/nautobot) on
RHEL/CentOS or Ubuntu servers.

## Requirements

This role manages the installation and configuration of Nautobot. This role
does not provide PostgreSQL or Redis services that are required dependencies
of the application. Those tasks are intentionally left to allow the user to 
manage those services within their own roles and playbooks.

Tested on Nautobot supported platforms:
* CentOS 8.2+ or Red Hat Enterprise Linux (RHEL) 8.2+
* Ubuntu 20.04

This role will require root access to manage system dependencies and actions
on behalf of Nautobot.

## Role Variables

Minimum required variables assuming `localhost` PostgreSQL and Redis services
are available:

    nautobot_db_username: nautobot
    nautobot_db_password: nautobot
    nautobot_secret_key: "lnvRn_5Bypl8hBV4mMwgsMuHxr6uZvGwJyDqB7fcKqo"

See [defaults/main.yml](defaults/main.yml) for a complete list of defaults and 
configurable options.

## User accounts

The following variables can be defined to create users during initial
installation only:

    nautobot_superusers:
      - username: admin
        password: admin
        email: changeme@example.com

Each user requires a username, password and email address defined. The role will
attempt to create the defined users only once during initial installation. If 
`nautobot_superusers` is not defined, no users are created and the manual user
creation process [documented](https://nautobot.readthedocs.io/en/latest/installation/nautobot/#create-a-superuser)
by Nautobot can be used instead.

## Dependencies

None.

## Example Playbook

    - hosts: nautobot
      
      roles:
        - jvoss.nautobot

*Wherever the hosts vars are defined*:

    nautobot_db_username: nautobot
    nautobot_db_password: securepass
    nautobot_db_host: postgres.server.url       # omit for default: localhost
    nautobot_redis_host: redis.server.url       # omit for default: localhost

    nautobot_superusers:     
      - username: admin                         # initial username
        password: admin                         # initial password
        email: admin@example.com
