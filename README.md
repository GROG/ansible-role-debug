# Debug

[![Ansible Galaxy](http://img.shields.io/badge/galaxy-GROG.debug-660198.svg?style=flat)](https://galaxy.ansible.com/GROG/debug)
[![Build Status](https://travis-ci.org/GROG/ansible-role-debug.svg?branch=master)](https://travis-ci.org/GROG/ansible-role-debug)

A role for managing a debug user, debug tools and debugging ansible variables.

It is strongly recommended to only use this role on development systems! Using
it in a production environment could result in serious security risks.

## Requirements

- Hosts should be bootstrapped for ansible usage (have python,...)
- Root privileges, eg `become: yes`
- `useradd`, `userdel` and `usermod` should be available on the host
- sudo should be available **(attention: this role will enable sudoers.d if not
  enabled)**

## Role Variables

| Variable | Description | Default value |
|----------|-------------|---------------|
| `debug_user` | Manage debug user? | `yes` |
| `debug_user_settings` | Settings for the debug user **(see details!)** | see details |
| `debug_tools` | Manage debug tools? | `yes` |
| `debug_tools_list` | List of debug tools **(see details!)** | `[]` |
| `debug_tools_list_host` | List of debug tools **(see details!)** | `[]` |
| `debug_tools_list_group` | List of debug tools **(see details!)** | `[]` |
| `debug_tools_state` | State of the debug tools | `present` |
| `debug_tools_update_cache` | Update package cache? | `yes` |
| `debug_tools_cache_valid_time` | Valid time for package cache | 3600 |
| `debug_variables` | Dump ansible variables for debugging? | `no` |
| `debug_variables_dump_location` | File to dump variables to | '/tmp/ansible.dump' |

#### `debug_user_settings` details

By default a user with following data will be created;

```yaml
debug_user_settings:
  name: debug
  comment: Ansible
  shell: '/bin/bash'
  authorized_keys:
    - key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"
  sudo:
    hosts: ALL
    as: ALL
    commands: ALL
    nopasswd: yes
```

It is however recomended to use your own custom user settings. More
information about the available attributes can be found in the documentation of
the GROG [user](https://galaxy.ansible.com/list#/roles/4730),
[authorized-key](https://galaxy.ansible.com/list#/roles/4737) and
[sudo](https://galaxy.ansible.com/list#/roles/4765) roles.

#### `debug_tools_list` details

`debug_tools_list`, `debug_tools_list_host` and `debug_tools_list_group` are
merged when managing the tools. You can use the host and group lists to specify
tools per host or group off hosts.

More information about the possible package attributes can be found in the
documentation of the GROG
[package](https://galaxy.ansible.com/list#/roles/4689) role.

## Dependencies

- [GROG.user](https://galaxy.ansible.com/list#/roles/4730)
- [GROG.authorized-key](https://galaxy.ansible.com/list#/roles/4737)
- [GROG.sudo](https://galaxy.ansible.com/list#/roles/4765)
- [GROG.package](https://galaxy.ansible.com/list#/roles/4689)
- [GROG.debug-variable](https://galaxy.ansible.com/list#/roles/4738)

## Example Playbook

```yaml
---
- hosts: debug
  roles:
  - { role: GROG.debug, become: yes }
```

## License

LGPLv3

## Contributing

All assistance, changes or ideas [welcome](https://github.com/GROG/ansible-role-debug/issues)!

## Author Information

By [G. Roggemans](https://github.com/groggemans)
