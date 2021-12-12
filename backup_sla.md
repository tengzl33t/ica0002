Coverage:
Databases backup only (MySQL and InfluxDB) or other services which can't be restored with ansible.

RPO:
Backup creates every day at 21:00-21:45 (12:00-21:15 for MySQL and 21:30-21:45 for InfluxDB) UTC preferably when nobody use services. Acceptable data loss: 1 day.

Retention:
Backups store for 2 weeks. Every sunday creates full backup, every other day only incremental.

Usability checks:
Twice a week, at tuesday and friday.

Restoration criteria:
When something is not working and can't be fixed easily without data restoring (DB data loss).

RTO:
Depends on the amount of data, usually takes 1 day.
