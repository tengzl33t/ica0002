Install and configure infrastructure with Ansible:

    ansible-playbook infra.yaml

Restore MySQL agama data from the backup:

    Open console with root user and type:

    rm -rf /home/backup/restore/*
    sudo -u backup duplicity --no-encryption restore rsync://tengzl33t@backup.sus.eu//home/tengzl33t/ /home/backup/restore/
    mysql agama < /home/backup/restore/agama.sql

Restore InfluxDB telegraf data from the backup:

    Open console with root user and type:

    systemctl stop telegraf
    influx -execute 'DROP DATABASE telegraf'
    influxd restore -portable -database telegraf /home/backup/influxdb

Check restore results:

    Make sure that restore commands didn't give errors,
    Check agama restore results at agama page,
    Check telegraf restore results at ??? somewhere

Verify that backup was successful and run ansible playbook again:

    ansible-playbook infra.yaml

