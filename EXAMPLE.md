# Playbook Example

The following provides a simple example of a complete Nautobot configuration using 
Ansible on Ubuntu 20.04. It is *not suitable for production* and is intended to only
demonstrate how the required components can be brought together.

## Install Roles

    $ ansible-galaxy install jvoss.nautobot geerlingguy.postgresql geerlingguy.redis hispanico.nginx_revproxy

## Host Variables

    # nautobo.yml

    nautobot_db_username: nautobot
    nautobot_db_password: password
    nautobot_secret_key: "-nit@y=2#)u2dz-e(de1$t*4mxpy4d9o(b4j5xf@6!ql=r-14o"

    nautobot_uwsgi_mode: http

    nautobot_superusers:        
      - username: admin
        password: admin
        email: admin@example.com

    nginx_revproxy_sites:    
      nautobot:
        domains:
          - nautobot.example.com
        upstreams:
          - { backend_address: localhost, backend_port: 8001 }
        ssl: false
        letsencrypt: false
        conn_upgrade: false

    postgresql_users:
      - name: "{{ nautobot_db_username }}"
        password: "{{ nautobot_db_password }}"
        db: "{{ nautobot_db_name }}"
    postgresql_databases:
      - name: "{{ nautobot_db_name }}"
        owner: "{{ nautobot_db_username }}"

## Playbook

    # playbook-nautobot.yml

    - hosts: nautobot
      become: yes

      roles:
        - role: geerlingguy.postgresql
        - role: geerlingguy.redis
        - role: jvoss.nautobot
        - role: hispanico.nginx_revproxy
