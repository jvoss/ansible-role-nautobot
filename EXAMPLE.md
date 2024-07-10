# Playbook Example

The following provides a simple example of a complete Nautobot configuration using 
Ansible. It is *not suitable for production* and is intended to only demonstrate how
the required components can be brought together.

At the completion of this play Nautobot will be available at: `http://<host_ip>:8080`

## Install Roles

    $ ansible-galaxy install jvoss.nautobot geerlingguy.postgresql geerlingguy.redis caddy_ansible.caddy_ansible

## Host variables

    # nautobot.example.com.yml

    nautobot_root: /opt/nautobot
    nautobot_db_username: nautobot
    nautobot_db_password: password
    nautobot_secret_key: "-nit@y=2#)u2dz-e(de1$t*4mxpy4d9o(b4j5xf@6!ql=r-14o"

    nautobot_uwsgi_mode: http

    nautobot_superusers:        
      - username: admin
        password: admin
        email: admin@example.com

    # caddy_ansible.caddy_ansible configuration
    caddy_config: |
      :8080 {
        route /static* {
          uri strip_prefix /static
          root * {{ nautobot_root }}/static
          file_server
        }

        reverse_proxy http://127.0.0.1:8001
      }

    # geerlingguy.postgresql configuration
    postgresql_users:
      - name: "{{ nautobot_db_username }}"
        password: "{{ nautobot_db_password }}"
        db: "{{ nautobot_db_name }}"
    postgresql_databases:
      - name: "{{ nautobot_db_name }}"
        owner: "{{ nautobot_db_username }}"

## Playbook

    # playbook-nautobot.yml

    - hosts: nautobot.example.com
      gather_facts: yes
      become: true

      roles:
        - role: geerlingguy.postgresql
        - role: geerlingguy.redis
        - role: jvoss.nautobot
        - role: caddy_ansible.caddy_ansible

## Notes

For testing on minimal installations be sure the following tools are available on the
host: `python3` `sudo` `ssh` `unzip` `tar`

CentOS minimal example:

    $ yum install python3 sudo openssh-server unzip tar
    $ service sshd start
