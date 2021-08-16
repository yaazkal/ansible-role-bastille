# ansible-role-bastille

An ansible role that helps configure a server as a [BastilleBSD](https://bastillebsd.org/) host for running containers (jails based) in [FreeBSD](https://www.freebsd.org/).

This is a work in progress ansible role. **Use it at your own risk**.

## Install this role

Simply run `ansible-galaxy install yaazkal.bastille` on your machine. Then integrate the role on your own playbook (see the example below).

## Requirements

This has been tested on FreeBSD 13.0 with Python installed (3.7 recommended).

## Role variables

This are the role variables and its defaults:

| Variable            | Default value | Description                                               |
|---------------------|---------------|-----------------------------------------------------------|
| bastille_zfs_enable |               | Set to YES to enable some ZFS magic (recommended)         |
| bastille_zfs_zpool  |               | The ZFS pool where Bastille will host its files and jails |
| bastille_ext_if     | vtnet0        | External network interface                                |
| bastille_releases   | 13.0-RELEASE  | List of releases to be available for jails                |
| bastille_timezone   | Etc/UTC       |                                                           |


Set them at your host_vars or host definition as you want it (see example).

## Dependencies

None.

## Example Playbook

A playbook can look like this:

```yaml
# File name: bastille_provision.yml
- name: "Initial configuration of the system"
  hosts: bastille
  roles:
    - yaazkal.bastille
```

An inventory file can look like this (this example overrides all default variables):

```yaml
# File name: hosts.yml
bastille:
  hosts:
    example.com:
      ansible_user: root
      bastille_zfs_enable: "YES"
      bastille_zfs_zpool: "zroot"
      bastille_ext_if: "vtnet0"
      bastille_releases:
        - 13.0-RELEASE
        - 12.2-RELEASE
      bastille_timezone: "America/Bogota"
```

Then you can run:

`ansible-playbook -i hosts.yml bastille_provision.yml`

## License

BSD 3 clause. See LICENSE file.

## Author Information

[@yaazkal](https://twitter.com/yaazkal) - Juan David Hurtado G.
