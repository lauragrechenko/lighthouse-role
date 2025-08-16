Lighthouse
=========

Deploys Lighthouse (ClickHouse web UI) and configures an Nginx site to serve it and proxy /clickhouse/ to a ClickHouse HTTP endpoint.

Requirements
------------

Ansible ≥ 2.12

OS: EL 8/9 (CentOS Stream/RHEL/Rocky/Alma)

Nginx installed and running (e.g., via a separate nginx_base role)

If SELinux is Enforcing:

Non-standard ports (≠80/443) must be allowed for Nginx to bind.

Custom docroot paths (outside default trees) must be labeled for httpd (httpd_sys_content_t).

Network/firewall/SG must allow the chosen listen port.


Role Variables
--------------

Defaults you can override (see defaults/main.yml):

lighthouse_vcs: "https://github.com/VKCOM/lighthouse.git"
lighthouse_version: "master"

# Static files docroot
lighthouse_location_dir: "/usr/share/nginx/html/lighthouse"

# Nginx site
nginx_conf_path: "/etc/nginx/conf.d/lighthouse.conf"
lighthouse_listen_port: 8080
server_name: "_"            # catch-all

# ClickHouse HTTP endpoint (proxied at /clickhouse/)
clickhouse_host: "127.0.0.1"
clickhouse_port: 8123


Dependencies
------------

Optionally: an Nginx role (e.g., nginx_base) to install & start Nginx before this role.

Example Playbook
----------------
```
- hosts: lighthouse
  become: true
  roles:
    - role: nginx_base               # optional
    - role: lighthouse
      vars:
        lighthouse_listen_port: 8080
        lighthouse_location_dir: "/usr/share/nginx/html/lighthouse"
        clickhouse_host: "10.0.0.42"
        clickhouse_port: 8123
        lighthouse_manage_firewalld: false   # if you open ports in cloud SG instead
```

License
-------

MIT

Author Information
-------

Laura
