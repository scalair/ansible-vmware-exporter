Ansible role : VMware exporter
=========

Installs and configures [VMware exporter](https://github.com/pryorda/vmware_exporter), an exporter used by [Prometheus](https://github.com/prometheus/prometheus).

Requirements
------------
No special requirements; note that this role requires root access, so has to be ran with `become: yes`


Role Variables
--------------
Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
vmware_exporter_port: 9272
```
The port on which the vmware_exporter http endpoint will be published.

```yaml
vmware_exporter_targets: []
```
This variable contains a list of sections (vsphere hosts) that the vmware_exporter will connect to.
It is based on the configuration file of the [VMware exporter](https://github.com/pryorda/vmware_exporter), so please refer to that before setting this variable.

Examples are provided below in 'Example Playbook' or in `defaults/main.yml`.

Dependencies
------------

- geerlingguy.repo-epel if you're using CentOS or a Red Hat derivative.


Example Playbook
----------------
```yaml
- hosts: vmware_exporter_servers
  become: yes
  vars:
    vmware_exporter_targets:
      - default:
          vsphere_host: "vcenter"
          vsphere_user: "user"
          vsphere_password: "password"
          ignore_ssl: False
          collect_only:
            vms: True
            snapshots: True
            vmguests: True
            datastores: True
            hosts: True
      - limited:
          vsphere_host: "slowvc.example.com"
          vsphere_user: "administrator@vsphere.local"
          vsphere_password: "password"
          ignore_ssl: True
          collect_only:
            vms: False
            snapshots: False
            vmguests: False
            datastores: True
            hosts: False

  roles:
    - role: geerlingguy.repo-epel
      when: ansible_os_family == 'RedHat'
    - jdelvecchio.vmware_exporter
```

License
-------

BSD

Author Information
------------------

This role was created in 2019 by Julien Delvecchio.
