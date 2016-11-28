# Ansible role: Gentoo_install

Performs an installation of Gentoo Linux against an InstallCD environment.

This role handles all steps required to install Gentoo Linux when run against
the InstallCD environment. It will partition, format/mount filesystems,
download/extract the stage tarball, configure locales and timezone, build a
kernel (using genkernel), install/configure syslog and cron daemons, install
grub, unmount filesystems, and reboot.

In order to use this role, you will need to boot the InstallCD image with
parameters like:

  gentoo dosshd passwd=some_root_pass

create a playbook:

  ---
  - hosts: all
    remote_user: root
    vars:
      # The 'portage' module breaks on py3, which is the default in the stage
      # tarball
      ansible_python_interpreter: /usr/bin/python2
    roles:
      - gentoo_install

and then run ansible with something like:

  $ ansible-playbook -i <IP address>, -e ansible_password=some_root_pass -e gentoo_install_hostname=myhostname gentoo_install.yml
