# Role Description

This Ansible role automates the setup of a Simple Signal IM proxy server

## Default Variables

For more information on default variables, please refer to the `defaults/main.yml` file.

## Installation

* __This role should be used together with ansible-galaxy collection bestiancode.nginx_ssl__
* Galaxy: https://galaxy.ansible.com/ui/repo/published/bestiancode/nginx_ssl/
* GitHub: https://github.com/BestianCode/ansible.role.signal_proxy#

You can install this collection and role using the `ansible-galaxy` command:

```bash
ansible-galaxy collection install bestiancode.nginx_ssl
ansible-galaxy role install bestiancode.signal_proxy
```

Alternatively, you can create a `requirements.yml` file and use it to install all collections and roles at once:

```bash
ansible-galaxy collection install -r requirements.yml --force
ansible-galaxy role install -r requirements.yml --force
```

Here's a sample `requirements.yml` file:

```yaml
collections:
  - name: bestiancode.sysadmin
  - name: bestiancode.nginx_ssl

roles:
  - name: bestiancode.signal_proxy
```

## Usage

Here's a sample `playbook`:

```yaml
- hosts:
    - all
  become: true
  tags:
    - basic
    - init
  roles:
    - bestiancode.sysadmin.initial_setup

- hosts:
    - web
  become: true
  tags:
    - nginx
  roles:
    # If collections are not defined here, it is mandatory to specify prefix bestiancode.nginx_ssl!
    - bestiancode.nginx_ssl.install
    - bestiancode.nginx_ssl.letsencrypt
    - bestiancode.nginx_ssl.reload
    # Add other roles here

- hosts:
    - web
  become: true
  tags:
    - nginx
  collections:
    # If collections are defined here...
    - bestiancode.nginx_ssl
  roles:
    # If you defined collections, prefix bestiancode.nginx_ssl is not needed.
    - install
    - letsencrypt
    - reload

- hosts:
    - signal
  become: true
  tags:
    - signal
  roles:
    - bestiancode.signal_proxy

#...
# - other constructions can be here
#...
```

## Parameters

```yaml
#
#
# Signal proxy URL will be: your.host.domain:signal_port
#                           vpn.example.com:4443
#
# If you use port 443, you have to disable catching of default port 443 by Nginx
# catch_default_443: false
# if not 443, you can use default catching
# catch_default_443: true
#
signal_port: 4443

signal_dns_name: "{{ ansible_host }}"
signal_ssl_name: "{{ signal_dns_name }}"
```

## URLs

- **GitHub**: https://github.com/BestianCode/ansible.role.signal_proxy
- **Ansible Galaxy**: https://galaxy.ansible.com/bestiancode/signal_proxy
