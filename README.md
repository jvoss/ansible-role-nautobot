# Ansible Role: Nautobot

[![CI](https://github.com/jvoss/ansible-role-nautobot/actions/workflows/ci.yml/badge.svg?branch=master)](https://github.com/jvoss/ansible-role-nautobot/actions/workflows/ci.yml)
[![Ansible Galaxy](https://img.shields.io/badge/galaxy-jvoss.nautobot-blue.svg)](https://galaxy.ansible.com/jvoss/nautobot)
[![Ansible Quality Score](https://img.shields.io/ansible/quality/55095?color=blue)](https://galaxy.ansible.com/jvoss/nautobot)
[![Version](https://img.shields.io/github/release/jvoss/ansible-role-nautobot.svg)](https://github.com/jvoss/ansible-role-nautobot/releases/)

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

This role will require root access (via sudo) to manage system dependencies and actions
on behalf of Nautobot.

## Role variables

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

See the [wiki](https://github.com/jvoss/ansible-role-nautobot/wiki) for
information about available external authentication methods.

## Plugins 

Nautobot plugins that are pip modules can be installed and configured by setting
the `nautobot_plugins` variable. Below is an example for the Nautobot
Nornir plugin:

      nautobot_plugins:
        - name: nautobot_plugin_nornir    # Plugin name
          pip: nautobot-plugin-nornir     # Pip module name
          config:                         # plugin configuration
            nornir_settings:
              credentials: "nautobot_plugin_nornir.plugins.credentials.env_vars.CredentialsEnvVars"
              runner:
                plugin: "threaded"
                options:
                  num_workers: 20
## Version locking

Optionally, a specific version of nautobot can be configured using the variable:

    nautobot_version: 1.0.1

This will ensure that a specific target is maintained. If not set, on initial
install pip will install the latest version. On subsequent runs it will only check that
the module is installed but will not upgrade it automatically.

In either case, an upgrade can be triggered by setting this variable to the desired
Nautobot release version. Downgrading is not supported.

## Dependencies

No Ansible dependencies. The application requires Redis and Postgres.

## Example Playbook

See [EXAMPLE](EXAMPLE.md) for a complete playbook example.

## Contributing

Contributions are encouraged. Please see [CONTRIBUTING](CONTRIBUTING.md) for
details.
