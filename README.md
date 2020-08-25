# ansible-role-bastille
An ansible role that helps configure a server as a BastilleBSD host

This is a work in progress ansible role. **Use it at your own risk**

## Role variables

This are the role variables and its defaults:

| Variable            | Default value | Description                                               |
|---------------------|---------------|-----------------------------------------------------------|
| bastille_zfs_enable |               | Set to YES to enable some ZFS magic (recommended)         |
| bastille_zfs_zpool  |               | The ZFS pool where Bastille will host its files and jails |
| bastille_ext_if     | vtnet0        | External network interface                                |
| bastille_release    | 12.1-RELEASE  |                                                           |
| bastille_timezone   | Etc/UTC       |                                                           |


Set them at your host_vars or host definition as you want it (see example).

## Example Playbook

A playbook can look like this:

```yaml
# File name: bastille_provision.yml
- name: "Initial configuration of the system"
  hosts: bastille
  roles:
    - yaazkal.bastille
```

A host file can look like this (this example overrides all default variables):

```yaml
# File name: hosts.yml
bastille:
  hosts:
    example.com:
      ansible_user: root
      bastille_zfs_enable: "YES"
      bastille_zfs_zpool: "zroot"
      bastille_ext_if: "vtnet0"
      bastille_release: "12.1-RELEASE"
      bastille_timezone: "America/Bogota"
```

Then you can run:

`ansible-playbook -i hosts.yml bastille_provision.yml`

## License

BSD 3 clause. See LICENSE file.