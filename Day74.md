## Day 74 – Jenkins Database Backup Job 🛠️💾

### 🔎 Goal
Automate **database backups** (MySQL/MariaDB/PostgreSQL) using Jenkins, store them locally or on a remote server, and optionally clean up old backups.  

---

## 🐧 Step 1: Create a Jenkins Job
1. Go to **Jenkins dashboard → New Item**  
2. Enter a name (e.g., `DBBackupJob`)  
3. Select **Freestyle Project** → **OK**  

---

## 🐧 Step 2: Add Parameters (Optional)
- **Database name**, **user**, or **backup path** can be added as **parameters**  
- Example:  
  - DB_NAME = `myappdb`  
  - DB_USER = `backupuser`  
  - BACKUP_DIR = `/opt/db_backups`  

---

## 🐧 Step 3: Add Build Step – Execute Shell

### MySQL/MariaDB Backup Script
```bash
#!/bin/bash

# Variables
DB_NAME="myappdb"
DB_USER="backupuser"
DB_PASS="StrongPassword"
BACKUP_DIR="/opt/db_backups"
TIMESTAMP=$(date +%F_%H-%M-%S)
BACKUP_FILE="$BACKUP_DIR/${DB_NAME}_$TIMESTAMP.sql.gz"

# Create backup directory if not exists
mkdir -p $BACKUP_DIR

# Backup database
mysqldump -u $DB_USER -p$DB_PASS $DB_NAME | gzip > $BACKUP_FILE

# Optional: copy to remote server
# scp $BACKUP_FILE user@remote_server:/opt/db_backups/

# Delete backups older than 10 days
find $BACKUP_DIR -type f -name "*.sql.gz" -mtime +10 -exec rm -f {} \;

echo "Backup completed: $BACKUP_FILE"
```
### PostgreSQL Backup Script
```bash
#!/bin/bash

DB_NAME="myappdb"
DB_USER="backupuser"
BACKUP_DIR="/opt/db_backups"
TIMESTAMP=$(date +%F_%H-%M-%S)
BACKUP_FILE="$BACKUP_DIR/${DB_NAME}_$TIMESTAMP.sql.gz"

mkdir -p $BACKUP_DIR

# Backup PostgreSQL database
PGPASSWORD="StrongPassword" pg_dump -U $DB_USER $DB_NAME | gzip > $BACKUP_FILE

# Delete backups older than 10 days
find $BACKUP_DIR -type f -name "*.sql.gz" -mtime +10 -exec rm -f {} \;

echo "Backup completed: $BACKUP_FILE"
```
