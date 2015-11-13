<img src="http://www.elao.com/images/corpo/logo_red_small.png"/>

[![Ansible Role](https://img.shields.io/ansible/role/5536.svg?style=plastic)](https://galaxy.ansible.com/list#/roles/5536) [![Platforms](https://img.shields.io/badge/platforms-debian-lightgrey.svg?style=plastic)](#) [![License](http://img.shields.io/:license-mit-lightgrey.svg?style=plastic)](#)

# Ansible Role: SSH

This role will assume the following configuration:
- Allow sudo authentication over ssh
- Enable/Disable the SSH daemon password authentication
- Set the SSH daemon accepted environment variables
- Set ssh know hosts

It's part of the ELAO <a href="http://www.manalas.com" target="_blank">Ansible stack</a> but can be used as a stand alone component.

## Requirements

- Ansible 1.9.0+

## Dependencies

None.

## Installation

Using ansible galaxy:

```bash
ansible-galaxy install elao.ssh,1.0
```
You can add this role as a dependency for other roles by adding the role to the meta/main.yml file of your own role:

```yaml
dependencies:
  - { role: elao.ssh }
```

## Role Handlers

|Name|Action|Description|
|----|----|-----------|-------|
`sudo restart`|Service restarted|Ensure sudo service has been restarted.
`ssh reload`|Service reloaded|Ensure ssh service has been reloaded.

## Role Variables

| Name                              | Default                  | Type       | Description                                        |
|---------------------------------- |------------------------- |----------- |--------------------------------------------------- |
| `elao_ssh_config_sshd_template`   | config/sshd/base.j2      | String     | Specify the template used to build sshd config     |
| `elao_ssh_config_template`        | config/base.j2           | String     | Specify the template used to build ssh config      |
| `elao_ssh_known_hosts`            | []                       | Collection | List of server known hosts.                        |
| `elao_ssh_known_hosts.host`       | -                        | String     | Name of the host                                   |
| `elao_ssh_known_hosts.key`        | omit                     | String     | Path to the signature                              |
| `elao_ssh_known_hosts.state`      | present                  | String     | If the key should be present or absent             |
| `elao_ssh_known_hosts.path`       | /etc/ssh/ssh_known_hosts | String     | Path of the file where the host key will be stored |

### SSH configuration

The `elao_ssh_config_sshd_template` is the template use to populate sshd configuration file. It's comming with different specific templates.

- base (Simple template with no default configuration)
- debian_wheezy (No surprise, this file is a copy of the Debian Wheezy default configuration file)
- dev (This configuration is for development)
- empty (You want to handle ALL configuration variables)
- test
- prod (For production purpose)

```yaml
elao_ssh_config_sshd_template: config/sshd/{{ _env }}.j2
```

### Dealing with known hosts file

The `elao_ssh_known_hosts` is a collection allowing to automatically add known hosts and avoid the prompt when you initiate a connection to a server for the first time.

```yaml
elao_ssh_known_hosts:
    - host:  github.com
      key:   "{{ lookup('file', playbook_dir ~ '/files/ssh/keys/github.com') }}"
      state: present
      path:  /etc/ssh/ssh_known_hosts
```

## Example playbook

    - hosts: servers
      roles:
         - { role: elao.ssh }

# Licence

MIT

# Author information

ELAO [**(http://www.elao.com/)**](http://www.elao.com)