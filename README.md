# Ansible Role: Nautobot

[![CI](https://github.com/jvoss/ansible-role-nautobot/actions/workflows/ci.yml/badge.svg?branch=master)](https://github.com/jvoss/ansible-role-nautobot/actions/workflows/ci.yml)

Installs and configures [Nautobot](https://github.com/nautobot/nautobot) on
RHEL/CentOS or Ubuntu servers.

## Requirements

This role manages the installation and configuration of Nautobot. Access to
remote or local PostgreSQL and Redis instances are required as a dependency of
the application itself.

Tested on:
* CentOS 8.2+ or Red Hat Enterprise Linux (RHEL) 8.2+
* Ubuntu 20.04

This role will require root access to manage system dependencies and actions
on behalf of Nautobot.

## Role Variables

Minimum required variables assuming `localhost` PostgreSQL and Redis services:

    nautobot_db_username: nautobot
    nautobot_db_password: nautobot
    nautobot_secret_key: "lnvRn_5Bypl8hBV4mMwgsMuHxr6uZvGwJyDqB7fcKqo"

    # Initial user to create during installation
    nautobot_superusers:
      - username: admin
        password: admin
        email: changeme@example.com

See [defaults/main.yml](defaults/main.yml) for a complete list of defaults and 
configurable options.

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
