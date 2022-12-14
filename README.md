# Ansible
Ansible Experimentation

Ansible is an automation engine that allows for agent-less system configuration and deployment.
Ansible operates over SSH and runs Ansible modules on remote system in order to complete tasks.

## Install
___
This install is specific to Red Hat Enterprise Linux

First you will need to install the epel-release repos.

`$ sudo dnf config-manager --enable codeready-builder-for-rhel-<VERSION>-rhui-rpms`

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
- This command will use the yum module to install the apache httpd software on the targeted host(s). 
- Forewarning Ansible will not assume root privileges unless specified to do so which is why I have applied the -b flag.

`ansible <HOST> -b -m service -a "name=httpd state=started"`
- This will change the state of httpd and should start the daemon on the specified host(s).
- The service module is designed to operate with daemons in general.

### Playbooks:
Playbooks run using the `ansible-playbook` command. If you compare Ansible commands to bash commands playbooks are synonymous with bash scripts.

Play books are written in YAML, They contain different elements called plays, plays contain list of hosts and at minimum one task. Each task has a name and module.

I recommend before running any playbook to first run it with the --check flag. This will create a dry run allowing you to see what Ansible would have changed, from this you can confirm the desired outcome.

`ansible-playbook <YAML FILE> --check`

To run a specific playbook this is the command you will use if using the default inventory file.

`ansible-playbook <YAML FILE>`

To run a playbook with a provided inventory file you will do so using the -i flag.

`ansible-playbook -i <INVENTORY FILE> <YAML FILE>`

If any of you plays fail to execute on a specific host, in example due to reachability then a playbook-name.retry file will be generated if RETRY_FILES_ENABLED is set to True. This file can be specified if re-running the playbook with the --limit flag to reattempt your plays on only the host which have failed.

`ansible-playbook <YAML FILE> --limit <RETRY FILE>`