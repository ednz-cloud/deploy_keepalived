deploy_keepalived
=========
> This repository is only a mirror. Development and testing is done on a private gitea server.

This role lets you install and configure keepalived on debian-based systems. It can either deploy it natively, or within a docker container.

Requirements
------------

None.

Role Variables
--------------
Available variables are listed below, along with default values.

```yaml
deploy_keepalived_deploy_method: host # by default, set to host
```
This variable defines the method of deployment of keepalived. The `host` method installs the binary directly on the host, and runs keepalived as a systemd service. The `docker` method install keepalived as a docker container.

```yaml
deploy_keepalived_version: "latest" # by default, set to latest
```
This variable sets the version of keepalived to install. Please note that not all versions are available. For the `host` deploy method, `2.2.3`, `2.2.4`, `2.2.7`, and `2.2.8` are officially supported (`2.2.0`,`2.2.1` and `2.2.2` require different compilation dependencies that I couldn't make work, while `2.2.5` and `2.2.6` are "false " releases, as no source code is available. Older versions might work, but are not tested.) For the `docker` deploy method, only `2.2.3`, `2.2.4`, `2.2.7`, and `2.2.8` are available. You can also use `2.2` or `2` on `docker` deployment, to stick to the latest minor or patch version. The `latest` is however recomended, and works on both `host` and `docker` deployment method.

```yaml
deploy_keepalived_start_service: true # by default, set to true
```
This variable defines if the keepalived service should be started once it has been configured. This is usefull in case you're using this role to build golden images, in which case you might want to only enable the service, to have it start on the next boot (when the image is launched).

```yaml
deploy_keepalived_env_variables: {} # by default, set to {}
```
This value is a list of key/value that will populate the keepalived.env (for `host` deployment method) or /etc/default/keepalived (for `docker` deploy method) file.

```yaml
deploy_keepalived_vrrp_instance_name: "{{ ansible_hostname }}" # by default, set to {{ ansible_hostname }}
```
This value sets the name of the vrrp instance. Defaults to the hostname of the machine.

```yaml
deploy_keepalived_interface: "{{ ansible_default_ipv4.interface }}" # by default, set to {{ ansible_default_ipv4.interface }}
```
This defines the interface for the vrrp instance. Defaults to the primary network interface.

```yaml
deploy_keepalived_state: "BACKUP" # by default, set to BACKUP
```
This variable sets the initial state of the vrrp instance. Defaults to backup, to avoid multi-master issues in cluster setups.

```yaml
deploy_keepalived_router_id: 50 # by default, set to 50
```
Arbitrary router id of the vrrp instance, defaults to 50

```yaml
deploy_keepalived_priority: 100 # by default, set to 100
```
Sets the initial priority of the instance.

```yaml
deploy_keepalived_advert_interval: 1 # by default, set to 1
```
Interval in second, at which the keepalived instance advertise itself to its peers.

```yaml
deploy_keepalived_unicast_source: "{{ ansible_default_ipv4.address }}" # by default, set to {{ ansible_default_ipv4 }}
```
The source ip to communicate with other vrrp peers. Defaults to the address of the primary interface, but isn't needed if no peers are set.

```yaml
deploy_keepalived_unicast_peers: [] # by default, set to []
```
List of unicast peers to advertise to. By default, no peers are set.

```yaml
deploy_keepalived_auth_passwd: "password"
```
The password for communicating to/from peers. You should really change it..

```yaml
deploy_keepalived_virtual_ips: # by default, set to ["192.168.1.100/32"]
  - 192.168.1.100/32
```
List of virtual IPs to set on the host if it has the MASTER state. Refer to the [documentation](https://manpages.debian.org/unstable/keepalived/keepalived.conf.5.en.html) for more information on the syntax.

```yaml
deploy_keepalived_notify_script: notify.sh # by default, set to notify.sh
```
Path to the notify script. The default one is located in `files/notify.sh`, but you can put any name here. This script is expected to be in `/etc/keepalived/scripts.d` on the host (keep reading to see how to copy your own scripts).

```yaml
deploy_keepalived_custom_scripts_src: # by default, unset
```
Path to a local scripts directory. The content of the directory will be copied over to `/etc/keepalived/scripts.d` on the host, and will override the default `notify.sh` script if it encounters name conflict (if you also copy a custom `notify.sh`). This lets you copy notify scripts, as well as any other scripts you may need.

```yaml
deploy_keepalived_extra_container_volumes: []
```
Extra volumes to mount to the container if using the `docker` deploy method.
By default, `/etc/keepalived` (host) will be mounted to `/etc/keepalived` (container)



Dependencies
------------

`ednz_cloud.manage_apt_packages` to install build dependencies for keepalived.

`ednz_cloud.docker_systemd_service` if installing keepalived in a container.

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
