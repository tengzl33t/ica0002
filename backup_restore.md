Install and configure infrastructure with Ansible:

    ansible-playbook infra.yaml

If you want to unite current data with previously saved backup:

    1. Save current data as incremental backup:
        For MySQL (run from read only machine!): 
            1. sudo -u backup mysqldump agama > /home/backup/mysql/agama.sql
            2. sudo -u backup duplicity --no-encryption incremental /home/backup/mysql/ rsync://tengzl33t@backup.sus.eu//home/tengzl33t/mysql/
            
        For InfluxDB: 
            1. sudo -u backup rm -rf /home/backup/influxdb/*; sudo -u backup influxd backup -portable -database telegraf /home/backup/influxdb/
            2. sudo -u backup duplicity --no-encryption incremental /home/backup/influxdb/ rsync://tengzl33t@backup.sus.eu//home/tengzl33t/influxdb/

    2. Continue with restoration quide below.

Restore MySQL agama data from the backup:

    Open console on MySQL Master (not read only!) machine with root user and type:

    1. rm -rf /home/backup/restore/mysql/*
    2. sudo -u backup duplicity --no-encryption restore rsync://tengzl33t@backup.sus.eu//home/tengzl33t/mysql /home/backup/restore/mysql/
    3. mysql agama < /home/backup/restore/mysql/agama.sql

Restore InfluxDB telegraf data from the backup:

    Open console with root user and type:

    1. systemctl stop telegraf
    2. influx -execute 'DROP DATABASE telegraf'
    3. sudo -u backup duplicity --no-encryption restore rsync://tengzl33t@backup.sus.eu//home/tengzl33t/influxdb/ /home/backup/restore/influxdb/
    4. influxd restore -portable -database telegraf /home/backup/restore/influxdb/

Check restore results:

    Make sure that restore commands didn't give errors,
    Check agama restore results at agama page,
    Check telegraf restore results at grafana syslog

Verify that backup was successful and run ansible playbook again:

    ansible-playbook infra.yaml
    or
    ansible-playbook infra.yaml --tags "influx"
    for telegraf restart

