# Ansible
Ansible Experimentation

Ansible is an automation engine that allows for agent-less system configuration and deployment.
Ansible operates over SSH and runs Ansible modules on remote system in order to complete tasks.

## Install
This install is specific to Red Hat Enterprise Linux

First you will need to install the epel-release repos.

`$ sudo dnf config-manager --enable codeready-builder-for-rhel-9-rhui-rpms`

`$ sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-<VERSION>.noarch.rpm`

`$ sudo dnf install ansible`
