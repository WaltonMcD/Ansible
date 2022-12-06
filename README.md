# Ansible
Ansible Experimentation

Ansible is an automation engine that allows for agent-less system configuration and deployment.
Ansible operates over SSH and runs Ansible modules on remote system in order to complete tasks.

## Install
___
This install is specific to Red Hat Enterprise Linux

First you will need to install the epel-release repos.

`$ sudo dnf config-manager --enable codeready-builder-for-rhel-9-rhui-rpms`

`$ sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-<VERSION>.noarch.rpm`

`$ sudo dnf install ansible`

## Quick Start 
___
### Authentication:
You may use ansible to connect to remote hosts via password authentication using the -k, but this is not common practice and can cause significant overhead in terms of manuel intervention.

Ansible is best implemented using a common user across all Ansible controlled systems. The best route for this access is by using pre-shared keys for user authentication through the `ssh-keygen` and `ssh-copy-id` commands.

Ansible will need root privileges be it either cart blanche access or if so desired by deep configuration of the sudoers file. You may also prompt for sudo access through the -K flag.

### Documentation:
The Ansible documentation can be found here. [Ansible](docs.ansible.com)

Detailed information on each module can be found here. [Ansible Modules](https://docs.ansible.com/ansible/2.9/modules/modules_by_category.html)

Ansible also ships with the `ansible-doc` command as well.
- Specifies a module names as a parameter and provides module specific documentation.
- Combined with the -l flag lists installed modules with short descriptions.

### Commands:
This section will cover some basic commands to be comfortable with ansible and declaring a proper installation.

`ansible <HOST> -b -m <MODULE> -a "<ARG1> <ARG2> <ARG3>"`
- HOST is a host or host group defined in the inventory file.
- -b is for become, which replaces the deprecated -s flag as in sudo
- -m is for module and allows a command to be used on a module
- -a allows parameters to pass. If used without a module it becomes synonymous with running a shell command on the target system.

`ansible <HOST> -m setup`
- This command will reach out to the targeted host(s) and will display all the information Ansible has collected on the host(s) using the setup module. Also known as Ansible facts.

`ansible <HOST> -m ping`
- This command will ping the targeted host(s) and either return a success or failure based upon reachability.

`ansible <HOST> -b -m yum -a "name=httpd state=latest"`
- This command will use the yum module to install the apache httpd software on the targeted host(s). Forewarning Ansible will not assume root privileges unless specified to do so which is why I have applied the -b flag.