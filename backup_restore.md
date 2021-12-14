# Restoration:
## Install and configure infrastructure with Ansible:

```ansible-playbook infra.yaml```

## (Optional) If you want to unite current data with previously saved backup:

1. Save current data as incremental backup:
    * For MySQL (run from read only machine!): 
        * ```sudo -u backup mysqldump agama > /home/backup/mysql/agama.sql```
        * ```sudo -u backup duplicity --no-encryption incremental /home/backup/mysql/ rsync://tengzl33t@backup.sus.eu//home/tengzl33t/mysql/```

    * For InfluxDB: 
        * ```sudo -u backup rm -rf /home/backup/influxdb/*; sudo -u backup influxd backup -portable -database telegraf /home/backup/influxdb/```
        * ```sudo -u backup duplicity --no-encryption incremental /home/backup/influxdb/ rsync://tengzl33t@backup.sus.eu//home/tengzl33t/influxdb/```

2. Continue with restoration guide below.

## Databases restoration:

**For opening console from root user type:
```sudo su```**

* Restore MySQL agama data from the backup:
    * Open console on MySQL Master (not read only!) machine with root user and type:

        * ```rm -rf /home/backup/restore/mysql/*```
        * ```sudo -u backup duplicity --no-encryption restore rsync://tengzl33t@backup.sus.eu//home/tengzl33t/mysql /home/backup/restore/mysql/```
        * ```mysql agama < /home/backup/restore/mysql/agama.sql```

* Restore InfluxDB telegraf data from the backup:

    * Open console on InfluxDB machine with root user and type:

        * ```systemctl stop telegraf```
        * ```influx -execute 'DROP DATABASE telegraf'```
        * ```sudo -u backup duplicity --no-encryption restore rsync://tengzl33t@backup.sus.eu//home/tengzl33t/influxdb/ /home/backup/restore/influxdb/```
        * ```influxd restore -portable -database telegraf /home/backup/restore/influxdb/```
        * ```systemctl start telegraf```

# Checking results:
## How to check restore results:

* Make sure that restore commands didn't give errors
* Check agama restore results at agama page or mysql database:
    * On MySQL machine from root user type: ```mysql -e 'SHOW DATABASES'```
* Check telegraf restore results in InfluxDB tables:
    * On InfluxDB machine from root user type: ```influx -execute 'show databases'``` 

## Verify that backup was successful and run ansible playbook again:

```ansible-playbook infra.yaml```
