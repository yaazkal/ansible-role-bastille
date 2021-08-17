# ansible-role-bastille

An ansible role that helps configure a server as a [BastilleBSD](https://bastillebsd.org/) host for running containers (jails based) in [FreeBSD](https://www.freebsd.org/).

This is a work in progress ansible role. **Use it at your own risk**.

## Install this role

Simply run `ansible-galaxy install yaazkal.bastille` on your machine. Then integrate the role on your own playbook (see the example below).

## Requirements

This has been tested on FreeBSD 13.0 with Python installed (3.7 recommended).

## Role variables

This are the role variables and its defaults, set them at your `host_vars` or host definition as you want it (see example).

| Variable            | Default value | Description                                                                                       |
|---------------------|---------------|---------------------------------------------------------------------------------------------------|
| bastille_version    |               | If set, installs the given version (tag) from bastille repo instead of the pkg version available. |
| bastille_zfs_enable |               | Set to YES to enable some ZFS magic (recommended).                                                |
| bastille_zfs_zpool  |               | The ZFS pool where Bastille will host its files and jails.                                        |
| bastille_timezone   | Etc/UTC       |                                                                                                   |
| bastille_ext_if     | vtnet0        | External network interface.                                                                       |
| bastille_releases   | 13.0-RELEASE  | List of releases to be available for jails creation.                                              |
| bastille_templates  |               | List of git repos where templates are hosted. Those templates will be available for jails.        |
| bastille_jails      |               | List of jails to be created. See example for options.                                             |

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
      bastille_version: "0.9.20210714"
      bastille_timezone: "America/Bogota"
      bastille_zfs_enable: "YES"
      bastille_zfs_zpool: "zroot"
      bastille_ext_if: "vtnet0"
      bastille_releases:
        - 13.0-RELEASE
        - 12.2-RELEASE
      bastille_templates:
        - https://gitlab.com/bastillebsd-templates/nginx
        - https://github.com/yaazkal/bastille-postgres
      bastille_jails:
        - name: thinjail # this is the default method in Bastille
          release: 13.0-RELEASE
          ip: 10.17.89.1
        - name: thickjail
          release: 13.0-RELEASE
          ip: 10.17.89.2
          options: -T
```

Then you can run:

`ansible-playbook -i hosts.yml bastille_provision.yml`

## License

BSD 3 clause. See LICENSE file.

## Author Information

[@yaazkal](https://twitter.com/yaazkal) - Juan David Hurtado G.
