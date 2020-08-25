# ansible-role-bastille
An ansible role that helps configure a server as a BastilleBSD host

This is a work in progress ansible role. **Use it at your own risk**

## Role variables

You **need** to set some variables for the host, please check the example. Later I can give defaults for flexibility.

## Example Playbook

A playbook can look like this:

```yaml
# File name: bastille_provision.yml
- name: "Initial configuration of the system"
  hosts: bastille
  roles:
    - yaazkal.bastille
```

A host file can look like this:

```yaml
# File name: hosts.yml
bastille:
  hosts:
    example.com:
      ansible_user: root
      timezone: "America/Bogota"
      bastille_zfs_enable: "YES"
      bastille_zfs_zpool: "zroot"
      bastille_release: "12.1-RELEASE"
```

Then you can run:

`ansible-playbook -i hosts.yml bastille_provision.yml`

## License

BSD 3 clause. See LICENSE file.