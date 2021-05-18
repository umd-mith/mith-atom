# mith-atom

This Ansible playbook is used to setup MITH's AtoM instance. It was adapted
from Artefactual's playbook at https://github.com/artefactual/deploy-pub

Someone in MITH (Ed Summers) will be able to provide you with the Ansible vault
password which allows you to decrypt needed passwords. Put a copy of the
password in a file named `vault.txt`.

Download the Ansible roles:

    ansible-galaxy install -r requirements.yml

Create the virtual machine and provision it:

    $ vagrant up

To ssh to the VM, run:

    $ vagrant ssh

If you want to forward your SSH agent too, run:

    $ vagrant ssh -- -A

To (re-)provision the VM, using Vagrant:

    $ vagrant provision

To (re-)provision the VM, using Ansible commands directly:

    $ ansible-playbook singlenode.yml
        --inventory-file=".vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory" \
        --user="vagrant" \
        --private-key=".vagrant/machines/atom-local/virtualbox/private_key" \
        --extra-vars="atom_dir=/vagrant/src atom_environment_type=development" \
        --verbose

To (re-)provision the VM, passing your own arguments to `Ansible`:

    $ ANSIBLE_ARGS="--tags=elasticsearch,percona,memcached,gearman,nginx" vagrant provision
