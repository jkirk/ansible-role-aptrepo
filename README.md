# aptrepo

![Lint Code Base](https://github.com/jkirk/ansible-role-aptrepo/actions/workflows/linter.yml/badge.svg)
![Ansible Molecule](https://github.com/jkirk/ansible-role-aptrepo/actions/workflows/molecule.yml/badge.svg)

A simple ansible role to set up Debian Apt repositories with GPG key support.

## Requirements

None.

## Default Role Variables

Role variables are set and documented in [`defaults/main.yml`](defaults/main.yml).

## Dependencies

None.

## Example Playbook

For this example we will assume you have defined a host group *megacli* in the inventory file `hosts`.

`site-aptrepo-hwraid.yml`


```yaml
---

- name: Install megacli from hwraid
  hosts: megacli
  become: yes
  vars:
    aptrepo_apt_key_url: "https://hwraid.le-vert.net/debian/hwraid.le-vert.net.gpg.key"
    aptrepo_apt_key_file: "hwraid.le-vert.net.asc"
    aptrepo_apt_repository: "http://hwraid.le-vert.net/debian"
    aptrepo_apt_release: "{{ ansible_distribution_release }}"
    aptrepo_apt_component: "main"
    aptrepo_apt_sources_file: "hwraid.list"
  roles:
    - role: aptrepo

  tasks:
    - name: "hwraid: Install megacli + Linux Kernel Headers"
      ansible.builtin.apt:
        name:
          - megacli
```

To run this playbook you would do `ansible-playbook -K -i hosts site-aptrepo-hwraid.yml`

## License

MIT

## Author Information

Darshaka Pathirana - <https://synpro.solutions>
