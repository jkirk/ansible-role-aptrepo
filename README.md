# aptrepo

![Lint Code Base](https://github.com/jkirk/ansible-role-aptrepo/actions/workflows/linter.yml/badge.svg)
![Ansible Molecule](https://github.com/jkirk/ansible-role-aptrepo/actions/workflows/molecule.yml/badge.svg)

A simple ansible role to set up Debian Apt repositories with GPG key support.

Requirements
------------

None.

Default Role Variables
----------------------

Role variables are set and documented in [`defaults/main.yml`](defaults/main.yml).

Dependencies
------------

None.

Example Playbook
----------------

For this example we will assume you have defined a host group *site* in the inventory file `hosts`.

`site-aptrepo-virtualbox.yml` (taken from https://github.com/jkirk/linux-desktop-bootstrap/blob/main/linux-desktop-virtualbox.yml)


```yaml
---

- name: Install VirtualBox (upstream) for Linux-Desktop
  hosts: localhost
  become: yes
  vars:
    virtualbox_version: "7.2"
    virtualbox_extpack_version: "7.2.6"
  roles:
    - role: aptrepo
      aptrepo_apt_key_url: "https://www.virtualbox.org/download/oracle_vbox_2016.asc"
      aptrepo_apt_key_file: "oracle_vbox_2016.asc"
      aptrepo_apt_repository: "https://download.virtualbox.org/virtualbox/debian"
      aptrepo_apt_release: "{{ ansible_distribution_release }}"
      aptrepo_apt_component: "contrib"
      aptrepo_apt_sources_file: "virtualbox.list"

  tasks:
    - name: "virtualbox: Install VirtualBox + Linux Kernel Headers"
      ansible.builtin.apt:
        name:
          - "virtualbox-{{ virtualbox_version }}"
          - "linux-headers-amd64"
        state: present
    # NOTE: VirtualBox Extension Pack needs a reboot or modprobe after VirtualBox has been installed
    - name: "virtualbox: Download VirtualBox Extension Pack"
      ansible.builtin.get_url:
        url: https://download.virtualbox.org/virtualbox/{{ virtualbox_extpack_version }}/Oracle_VirtualBox_Extension_Pack-{{ virtualbox_extpack_version }}.vbox-extpack
        dest: /tmp/Oracle_VirtualBox_Extension_Pack-{{ virtualbox_extpack_version }}.vbox-extpack
        owner: root
        group: root
        mode: 0644
    - name: "virtualbox: Install VirtualBox Extension Pack"
      ansible.builtin.shell: VBoxManage extpack install --accept-license=eb31505e56e9b4d0fbca139104da41ac6f6b98f8e78968bdf01b1f3da3c4f9ae --replace /tmp/Oracle_VirtualBox_Extension_Pack-{{ virtualbox_extpack_version }}.vbox-extpack

```

To run this playbook you would do `ansible-playbook -K -i hosts site-aptrepo-virtualbox.yml`

License
-------

MIT

Author Information
------------------

Darshaka Pathirana - <https://synpro.solutions>
