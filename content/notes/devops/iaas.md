+++
title = 'Infra as a Service'
date = 2024-02-11

+++

### IaaS Basics

- **Orchestration tools** are used to provision, organize, and manage infrastructure components.
- **Configuration management tools** are used to install, update, and manage the software running on the infrastructure components.
- Additionally, when it comes to IaC, there are two primary approaches to managing infrastructure:
  - Some tools (e.g., SaltStack) treat infrastructure components as mutable objects. That is, every time you make a change to the configuration, the necessary set of changes are made to the already-running components.
  - Other tools (e.g., Terraform) treat infrastructure components as immutable objects. That is, when you make a change, new components with the new configuration are created to replace the old components.
- **Continuous Integration (CI)** is the practice of team members merging their code changes into a mainline branch multiple times per day.
- **Continuous Delivery (CD)** is the process of making regular automated releases every time code is pushed to the mainline branch.
- With IaC, you can set up a deployment pipeline that automates the process of moving different versions of your application from one environment to the next as your testing and release schedules dictate

### Ansible

- Inventory : a file where we keep list of all our servers
- Modules : pluggable pieces of code. e.g apt module
- Tasks : define what actions ansible should do
- Playbook : contains instructions ususally applicable to the host
- Roles :
- If ansible modules are the tools in your workshop, playbooks are your instruction manuals, and your inventory of hosts are your raw material
- `ansible -i inventory_file devops -m ping -u $user`
- `ansible -i inventory_file devops -m command -a "hostname" -u $user`
- changed=1 means changed
- ansible-playbook -i inventory playbook-arch.yml
- When ansible was giving UNREACHABLE error : run `ssh-copy-id vagrant@ip_address` to copy the public key to the VMs.

### Vagrant

- A tool to build and manage virtual machine environments
- In vagrant a box means an image. For e.g Debian 10 is an image
- `vagrant init` generates a Vagrantfile
- `vagrant ssh` : to login to vm using ssh
- `vagrant ssh-config` : shows the ssh-config used by vagrant to login
- `vagrant halt` : to power off the vm
- provision the vm means some commands to execute when vm is coming up
- `vagrant reload --provision` : it will run the vm with provisioning steps
- `vagrant up --provision` : it will up the machine and provision them
- `vagrant destroy` : it will destroy the vm
- `vagrant ssh $vm_name` : to ssh into the given vm when using multiple VMs
- `vagrant box list`
- `vagrant box remove hashicorp/bionic641`
- The url to hit by nginx nodejs.devops.esc.sh which in turn directs to backend servers running on 192.168.30.11 & 12
