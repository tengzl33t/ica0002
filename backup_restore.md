Install and configure infrastructure with Ansible:

    ansible-playbook infra.yaml

Restore MySQL data from the backup:

    su - backup duplicity --no-encryption restore rsync://{{ github_username }}@backup.{{ domain }}.{{ tld }}//home/elvis/ /home/backup/restore/
    
    <another-command>
    <yet-another-command>

<add a few words here how the result of backup restore can be checked>