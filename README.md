# mith-atom

This Ansible playbook is used to setup MITH's
[AtoM](https://www.accesstomemory.org/en/) instance. It was adapted from
Artefactual's playbook at https://github.com/artefactual/deploy-pub

To get started you will need to clone this repository:

    git clone https://github.com/umd-mith/mith-atom.git

## Passwords

Someone in MITH (Ed Summers?) will be able to provide you with the Ansible
vault password which allows you to decrypt needed passwords. Put a copy of the
password in a file named `vault.txt`.

If you are adapting this playbook for other purposes you will want to save your
own password in `vault.txt` and then overwrite the contents of
`group_vars/atom/vault.yml` so that it contains (after changing the passwords):

```yaml
atom_config_db_password: "CHANGME" 
mysql_root_password: "CHANGEME"
atom_user_password: "CHANGEME"
```

Then you can encrypt your passwords with 
[ansible-vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html):

    ansible-vault encrypt group_vars/atom/vault.yml

## Provisioning

Prior to running the playbook you will need to provision an instance of Ubuntu
and put the hostname in the `hosts` file. MITH currently use an AWS `t3.large`
instance with 100GB of disk running Ubuntu 20.04 (Focal Fossa). Be sure to
choose an Intel based processor rather than ARM since Percona does not install
correctly on ARM.

You may want to update the `hostname` variable in `group_vars/atom/vars.yml` to
point to your new hostname. Currently it is set to `atom.mith.us`.

Once the host is up and the DNS record is available you can run the playbook.
DNS resolution is needed because the playbook will use LetsEncrypt to request
a certificate for your hostname.

## Running

First install Python requirements for running Ansible:

    pip3 install -r requirements.txt 

Then install the Ansible roles that are needed:

    ansible-galaxy install -r requirements.yml

The first time you run the playbook against a host you need to tell the
[ansible-atom](https://github.com/artefactual/ansible-atom) playbook to
initialize the database:

    ansible-playbook atom.yml --extra-vars "atom_auto_init=yes"

Artefactual recommend that subsequent runs of the playbook should not pass the
`atom-auto_init` variable, so you can just run it this way afterwards:

    ansible-playbook atom.yml

After the playbook has finished open the hostname in your browser and you
should see a running instance of AtoM!
