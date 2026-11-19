\# Automated Backup and Recovery System



\## 1. Overview



This document describes the backup and recovery system implemented for a self-hosted AFFiNE deployment running on Ubuntu 24.04.



The backup system was designed to protect application data and provide a recovery method in the event of database corruption, accidental deletion, or server failure.



The implementation uses Bash scripts, PostgreSQL database dumps, cron scheduling, and an external SSD.



\## 2. Backup Architecture



The backup workflow consists of three primary components:



1\. \*\*Application backup:\*\* Creates backups of the PostgreSQL database, application files, and configuration.

2\. \*\*Backup synchronization:\*\* Copies backup data to an externally connected SSD.

3\. \*\*Scheduled execution:\*\* Uses cron to automate the backup process.



The external SSD provides a separate storage location from the server's internal drive.



However, because the SSD remains locally connected, this is not a complete offsite disaster recovery solution.



\## 3. Backup Technologies



| Technology | Purpose |

|---|---|

| Bash | Backup automation scripts |

| PostgreSQL | Application database |

| `pg\_dump` | Database backup generation |

| `rsync` | File synchronization |

| Cron | Scheduled execution |

| External SSD | Local backup storage |

| GPG | Encryption of recovery archives |



\## 4. PostgreSQL Database Backups



The AFFiNE deployment uses PostgreSQL 16.



Database backups are generated using PostgreSQL's custom-format dump capability.



An illustrative command is:



```bash

pg\_dump -U "$POSTGRES\_USER" -d "$POSTGRES\_DB" -Fc -f backup.dump

```



The custom format supports restoration using `pg\_restore`.



In the actual deployment, the backup process is automated through a Bash script rather than requiring manual database exports.



Database credentials and production configuration values are excluded from this repository.



\## 5. Application and Configuration Backups



In addition to PostgreSQL database records, the backup process includes:



\- AFFiNE application files.

\- Application configuration data.

\- Files needed to support recovery of the deployment.



Backing up both database records and application files is important because a database dump alone may not contain every file required to restore the application.



Sensitive configuration files are included in the private backup process but are not committed to this public GitHub repository.



\## 6. External SSD Backup Storage



A 1 TB external SanDisk SSD is used for local backup storage.



The drive is formatted with the Linux ext4 filesystem and mounted on the Ubuntu server.



A Bash script uses `rsync` to synchronize backup data to the external storage.



An illustrative command is:



```bash

rsync -a --delete /path/to/source/ /path/to/backup-destination/

```



\*\*Caution:\*\* The `--delete` option removes destination files that no longer exist in the source. It should only be used after carefully verifying the source and destination paths.



The production backup script uses server-specific paths that are intentionally excluded from this public documentation.



\## 7. Automated Backup Scheduling



The backup synchronization process is scheduled through the root user's crontab.



The configured schedule runs daily at \*\*2:00 AM Central Time\*\*, using the server's America/Chicago timezone.



An example cron schedule is:



```cron

0 2 \* \* \* /path/to/usb-backup.sh

```



This schedule executes the script at 2:00 AM according to the cron daemon's configured timezone.



The use of cron allows backup operations to run without manual intervention, provided the server and external storage are available.



\## 8. Backup Verification



Backup verification is an important part of the recovery strategy.



Verification activities performed during the project included:



\- Confirming that backup files were created.

\- Confirming that backup data was present on external storage.

\- Inspecting the PostgreSQL backup output.

\- Performing a PostgreSQL database restore test.



The successful database restore test provided evidence that the database dump could be restored.



A complete end-to-end disaster recovery test of the entire AFFiNE environment remains a future improvement.



\## 9. Recovery Considerations



A recovery process would generally involve:



1\. Preparing a compatible Linux and Docker environment.

2\. Restoring the AFFiNE application configuration.

3\. Restoring application files.

4\. Restoring the PostgreSQL database using `pg\_restore`.

5\. Starting the application containers.

6\. Checking logs and verifying application functionality.



The exact procedure depends on the AFFiNE version, database configuration, and backup contents.



Recovery should be tested in an isolated environment before being relied upon during a production incident.



\## 10. Encrypted Recovery Archives



GPG encryption was also used to create encrypted recovery archives.



Encryption helps protect sensitive application and configuration data if backup media is lost or accessed without authorization.



The archive encryption key or passphrase must be protected separately from the backup media.



An encrypted archive should not be considered verified until its existence, integrity, and decryption process have been tested.



\## 11. Limitations and Future Improvements



The current backup strategy provides useful local recovery capabilities, but several improvements remain:



\- Implement offsite or geographically separate backups.

\- Introduce backup failure notifications.

\- Add automated backup integrity checks.

\- Test complete application restoration.

\- Document recovery time and recovery point objectives.

\- Periodically test encrypted archive decryption.

\- Consider maintaining multiple historical backup versions.



These improvements would help strengthen resilience against hardware failure, accidental deletion, and site-level incidents.



\## 12. Skills Demonstrated



This portion of the project demonstrates practical experience with:



\- Bash scripting.

\- Linux cron scheduling.

\- PostgreSQL administration.

\- Database backup and restore operations.

\- `rsync` synchronization.

\- Linux filesystem management.

\- External storage configuration.

\- Backup verification.

\- Disaster recovery planning.



\---



\*\*Security note:\*\* This repository documents the backup architecture without publishing production scripts, database dumps, private configuration files, encryption passphrases, or sensitive business data.

