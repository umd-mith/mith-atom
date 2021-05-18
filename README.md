# mith-atom

This Ansible playbook is used to setup MITH's AtoM instance. It was adapted
from Artefactual's playbook at https://github.com/artefactual/deploy-pub

Someone in MITH (Ed Summers) will be able to provide you with the Ansible vault
password which allows you to decrypt needed passwords. Put a copy of the
password in a file named `vault.txt`.

    ansible-galaxy install -r requirements.yml
    ansible-playbook atom.yml
