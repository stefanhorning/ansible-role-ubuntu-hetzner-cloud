# ubuntu-hetzner-cloud

Ansible role for bootstrapping a Hetzner Ubuntu cloud image with useful tooling for that Cloud offering

[Hetzner](https://www.hetzner.de/cloud) provides super cheap cloud server instances. However the offering
is much more basic then other cloud providers suche as AWS, Digital Ocean etc.

The limitations are twofold:
1. Their provided Ubuntu images are quite basic, just havin a root user, which is used for SSH signin (no other users, more secure SSH config)
2. No Firewall product that let's you easily manage instance access through the Cloud offering itself (such as AWS seucrity groups or DO firewalls)

To leviate both of this shortcomings (and alos install other nice tooling) this role exits.
It will do the following:

1. Setup a normal linux system user which then can be used for connecting (sudo enabled)
2. Disable root login and PW login for SSH
3. Installs UFW firewall and locks down all ports by default, only leaving SSH port open

Additionally this role provides those features:
1. Move SSH port is moved to a non default port
2. Upgrade all apt packages
3. Install a bunch of useful server command line tools (htop, iptraf etc.)

For providing parameters see below.

## Requirements

Ubuntu server bootet from Hetzner image. Probably anything newer then 18.04 is fine

## Role Variables

Overwritable role default variables:

- `hetzner_cloud_custom_user: ubuntu` - linux system user to setup (to not use root for everything, like ssh)
- `hetzner_cloud_update_apt: true` - run a full update upgrade, useful after brining up fresh instance
- `hetzner_cloud_ssh_port: 2222` - put SSH onto different port (prevent scanning), this is respected in Firewall settings
- `hetzner_cloud_open_ports: [{ name: http, port: 80 }]` - List of extra port to open (SSH will always be allowed, so you don't lock youself out)

For more see `defaults/main.yml` file.

## Dependencies

This role depends on no other roles.

## Example Playbook

Use role like this in your playbook, after installing this role:

```yaml
- name: Install server stuff
  hosts: cloud-servers
  vars:
    # vars for stefanhorning.ubuntu_hetzner_cloud role:
    hetzner_cloud_custom_user: ubuntu
    hetzner_cloud_update_apt: true
    hetzner_cloud_ssh_port: 2222
    hetzner_cloud_open_ports:
      - { name: http, port: 80 }
      - { name: https, port: 443 }
  roles:
    - stefanhorning.ubuntu_hetzner_cloud
  tasks:

    - name: Do other stuff
      ping:
```

## License

BSD

## Author Information
Stefan Horning
