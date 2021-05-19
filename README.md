# mith-atom

This Ansible playbook is used to setup MITH's AtoM instance. It was adapted
from Artefactual's playbook at https://github.com/artefactual/deploy-pub

Someone in MITH (Ed Summers) will be able to provide you with the Ansible vault
password which allows you to decrypt needed passwords. Put a copy of the
password in a file named `vault.txt`.

Prior to running the playbook you will need to provision an instance of Ubuntu
and put the hostname in the `hosts` file. You will also want to update the
`hostname` variable in `group_vars/atom/vars.yml` to point to your new
hostname. Currently it is set to `atom.mith.us`.

Once the host is up and the DNS record is available you can run the playbook.
DNS resolution is needed because the playbook will use LetsEncrypt to request
a certificate for your hostname.

Install python requirements (Ansible):

    pip3 install ansible

Install Ansible roles that are needed:

    ansible-galaxy install -r requirements.yml

The first time you run the playbook against a host you need to tell the
[ansible-atom](https://github.com/artefactual/ansible-atom) playbook to
initialize the database:

    ansible-playbook atom.yml --extra-vars "atom_auto_init=yes"

Artefactual recommend that Subsequent runs to the playbook should not pass
the extra variable:

    ansible-playbook atom.yml

After the playbook has finished running look at the hostname in your browser and you should see a running instance of AtoM!
