# aptrepo

![Lint Code Base](https://github.com/jkirk/ansible-role-aptrepo/actions/workflows/linter.yml/badge.svg)
![Ansible Molecule](https://github.com/jkirk/ansible-role-aptrepo/actions/workflows/molecule.yml/badge.svg)

A simple ansible role to set up Debian Apt repositories with GPG key support.

This role avoids using `apt-key add -` and the `ansible.builtin.apt_key` module
and follows Debian best-practices on how to use third-party Debian repository
signed keys. See:

* <https://michael-prokop.at/blog/2021/02/16/how-to-properly-use-3rd-party-debian-repository-signing-keys-with-apt/>
* <https://wiki.debian.org/DebianRepository/UseThirdParty#Sources.list_entry>

This role automatically dearmors the GPG file and adds the suffix .gpg to the
given `aptrepo_apt_key_file`, because older apt versions (older than 1.4)
needed a GPG key public ring file (AKA binary OpenPGP format).

This role also support some LMDE codenames and automatically converts them to
the Debian codename for `ansible_distribution_release`.

See `_aptrepo_release_map` in [`defaults/main.yml`](defaults/main.yml).

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
    aptrepo_apt_key_file: "hwraid.le-vert.net"
    aptrepo_apt_repository: "http://hwraid.le-vert.net/debian"
    aptrepo_apt_release: "{{ ansible_distribution_release }}"
    aptrepo_apt_component: "main"
    aptrepo_apt_sources_file: "hwraid.list"
  roles:
    - role: jkirk.aptrepo

  tasks:
    - name: "hwraid: Install megacli"
      ansible.builtin.apt:
        name:
          - megacli
```

To run this playbook you would do `ansible-playbook -K -i hosts site-aptrepo-hwraid.yml`

## License

MIT

## Author Information

Darshaka Pathirana - <https://synpro.solutions>
