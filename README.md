# ansible-role-bastille

An ansible role that helps configure a server as a [BastilleBSD](https://bastillebsd.org/) host for running containers (jails based) in [FreeBSD](https://www.freebsd.org/).

This is a work in progress ansible role. At the moment assuming local interface for networking. **Use it at your own risk**.

## Install this role

Simply run `ansible-galaxy install yaazkal.bastille` on your machine. Then integrate the role on your own playbook (see the example below).

## Requirements

* A current supported release of FreeBSD. See [supported releases](https://www.freebsd.org/security/#sup)
* Python installed on the target host.
* `ca_root_nss` recommended specially on FreeBSD 11.4 (EOL) in order to not fail when installing custom Bastille version from github tag.

## Role variables

This are the role variables and its defaults, set them at your `host_vars` or host definition as you want it (see example).

| Variable            | Default value       | Description                                                                                       |
|---------------------|---------------------|---------------------------------------------------------------------------------------------------|
| bastille_version    |                     | If set, installs the given version (tag) from bastille repo instead of the pkg version available. |
| bastille_prefix     | /usr/local/bastille | Where jails, releases, templates, backpus etc lives.                                              |
| bastille_zfs_enable |                     | Set to YES to enable some ZFS magic (recommended).                                                |
| bastille_zfs_zpool  |                     | The ZFS pool where Bastille will host its files and jails.                                        |
| bastille_timezone   | Etc/UTC             |                                                                                                   |
| bastille_ext_if     | vtnet0              | External network interface.                                                                       |
| bastille_releases   | 13.2-RELEASE        | List of releases to be available for jails creation.                                              |
| bastille_templates  |                     | List of git repos where templates are hosted. Those templates will be available for jails.        |
| bastille_jails      |                     | List of jails to be created. See example for options.                                             |

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
        - 13.2-RELEASE
        - 12.4-RELEASE
      bastille_templates:
        - https://gitlab.com/bastillebsd-templates/nginx
        - https://github.com/yaazkal/bastille-postgres
      bastille_jails:
        - name: defaultjail
          release: 13.2-RELEASE
          ip: 10.17.89.1
          templates:
            - template: "yaazkal/bastille-postgres"
              args:
                - version=12
            - template: "yaazkal/bastille-matomo"
              args:
                - version=2
                - other=yes
                - user=whatever
        - name: thickjail
          release: 13.2-RELEASE
          ip: 10.17.89.2
          options: -T
```

Then you can run:

`ansible-playbook -i hosts.yml bastille_provision.yml`

## License

BSD 3 clause. See LICENSE file.

## Author Information

[@yaazkal](https://twitter.com/yaazkal) - Juan David Hurtado G.
