deploy_keepalived
=========

A brief description of the role goes here.

Requirements
------------

None.

Role Variables
--------------
Available variables are listed below, along with default values. A sample file for the default values is available in `default/deploy_keepalived.yml.sample` in case you need it for any `group_vars` or `host_vars` configuration.

```yaml
your_defaults_here: default_value # by default, set to default_value
```
A quick description of the variable, what it does, and how to use it.

Dependencies
------------

None.

Example Playbook
----------------

```yaml
# calling the role inside a playbook with either the default or group_vars/host_vars
- hosts: servers
  roles:
    - ednz_cloud.deploy_keepalived
```

License
-------

MIT / BSD

Author Information
------------------

This role was created by Bertrand Lanson in 2024.
