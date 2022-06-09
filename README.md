Role Name
=========

Ansible role to handle certificates for IPSec encryption

Requirements
------------

This role assumes you are making use of https://github.com/linux-system-roles/vpn to setup IPSec tunnels, using certificate auth. It also assumes the host has been enrolled in a IPA/FreeIPA domain. This role then fetches the CA and host certificates from an IPA server, setting up the /etc/ipsec.d NSS certificate database.

Role Variables
--------------

No additional configuration should be required; however, there are some options in defaults/main.yml, like keysize.

Dependencies
------------

- https://github.com/linux-system-roles/vpn configured for IPSec
- IPA/FreeIPA enrollment

Example Playbook
----------------
```
- hosts: all
  collections:
    - fedora.linux_system_roles
  vars_files:
    - 'vault/vault.yaml'
    - 'vars/vars.yaml'
  gather_facts: true
  vars:
    vpn_ensure_openssl: false
    vpn_connections:
      - opportunistic: true
        auth_method: cert
        policies:
          - policy: clear
            cidr: default
          - policy: private
            cidr: 172.16.25.0/24
  tasks:
    - import_role:
        name: vpn

    - import_role:
        name: sirwalrus.ipsec-certs
```
License
-------

GNU General Public License v3.0

Author Information
------------------

Brian J. Atkisson

