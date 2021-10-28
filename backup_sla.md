Coverage:
Databases backup only (MySQL and InfluxDB), or other services which can't be restored with ansible.

RPO:
Backup every day at 03:00 am preferably when nobody use services. Acceptable data loss: 1 day.

Retention:
Backups store for 1 week, so 7 versions 1 for each day of week.

Usability checks:
Twice a week, at tuesday and friday.

Restoration criteria:
When something is not working and can't be fixed easily without data restoring (DB data loss).

RTO:
Depends on the amount of data, usually takes 1 day.
