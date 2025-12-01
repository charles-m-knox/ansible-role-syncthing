# Syncthing

Ansible role that builds, installs, and configures Syncthing instances.

When using `systemd`, user-scoped services will be installed. `openrc` is
supported as well, but not tested since 2023 and it isn't guaranteed
to work at a user-scoped level.

## Requirements

There are no requirements for this, it should work out of the box with Ansible.

## Role Variables

Please see `defaults/main.yml` for variables that can all be set on a per-host
basis.

## Dependencies

Your hosts must have Go installed.

## Usage

First, create a `requirements.yml` in your Ansible setup if you haven't already,
here's an example:

```yaml
---
collections:
  - name: community.general
  - name: ansible.posix
  - name: community.crypto

roles:
  - name: charles-m-knox.syncthing
    src: https://github.com/charles-m-knox/ansible-role-syncthing.git
    scm: git
    version: main
```

Next, install it:

```bash
# include the -f flag to forceably re-install and get the latest (equivalent to
# upgrading)
ansible-galaxy role install -f -r requirements.yml
```

Now, in your `site.yml` (or whatever your playbook is named), use the role -
note that root access is required for some of the steps:

```yaml
- name: syncthing
  hosts: all
  roles:
    - charles-m-knox.syncthing
```

For the most important part: in your host(s), define a `syncthing_instances`
list like this:

```yaml
syncthing_instances:
  - name: main
    data_dir: "{{ ansible_facts['env']['HOME'] }}/syncthing"  # will be created
    home_dir: "{{ ansible_facts['env']['HOME'] }}/.config/syncthing"  # will be created
    args: "--gui-address=127.0.0.1:8384"
```

Finally, run your playbook, optionally stepping through one-by-one:

```bash
ansible-playbook site.yml --tags syncthing --step
```

## License

MIT

## Author Information

<https://charlesmknox.com>
