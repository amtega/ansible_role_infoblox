# Ansible amtega.infoblox role

This is an [Ansible](http://www.ansible.com) role to setup host records on Infoblox NIOS.

Currently the role only reserves one ip for each vlan. If host is already provisioned, the record with the ips for each desired vlan is returned. If any vlan has no ip, 'not found' is
returned in ip field.

## Requirements

Python infoblox-client on the host that executes this rol/playbook. You can install with `pip install infoblox-client`

## Role Variables

A list of all the default variables for this role is available in `defaults/main.yml`.

The role also setups the following facts:

- `infoblox_hosts_records`: list of dicts with the records added to infoblox, indicating the IP address reserved for each VLAN.
- `infoblox_deleted_hosts_records`: list of dicts with the records removed from infoblox

## Usage

This is an example playbook:

```yaml
---

- hosts: all
  roles:
    - role: amtega.infoblox
      vars:
        infoblox_hosts:
          - name: ansible_test1.acme.com
            state: present
            dns: false
            vlans:
              - vlanExample1
              - vlanExample2
          - name: ansible_test2.acme.com
            state: present
            dns: false
            vlans:
              - vlanExample1
              - vlanExample2

        infoblox_vlans:
          - name: vlanExample1
            cidr: 24
            gateway: 10.8.1.1

          - name: vlanExample2
            cidr: 24
            gateway: 10.9.1.1

        infoblox_nios_provider:
          host: puthere.yourserver.name
          username: admin_user
          password: changeme
```

## Testing

Tests are based on [molecule with docker containers](https://molecule.readthedocs.io/en/latest/installation.html).

Tests need and infoblox NIOS working server and settings. So, to run the tests you have to pass the following variables:

- `ANSIBLE_INVENTORY`: path to an inventory providing the variables required by the role
- `ANSIBLE_VAULT_PASSWORD_FILE`: path to the file containing the vault password required for the previous inventory (optional)

```shell
cd amtega.infoblox

ANSIBLE_INVENTORY=~/myinventory ANSIBLE_VAULT_PASSWORD_FILE=~/myvaultpassword molecule test --all
```

## License

Copyright (C) 2022 AMTEGA - Xunta de Galicia

This role is free software: you can redistribute it and/or modify it under the terms of:

GNU General Public License version 3, or (at your option) any later version; or the European Union Public License, either Version 1.2 or – as soon they will be approved by the European Commission ­subsequent versions of the EUPL.

This role is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more details or European Union Public License for more details.

## Author Information

- José Enrique Mourón Regueira
