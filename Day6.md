## Day 6 – Create a Cron Job 🕒🐳

### 🎯 Task
- Backup Docker container data from **Server A**  
- Restore (copy) it to **Server B**  
- Delete backups older than **10 days**  

---

### 🛠️ Backup Script (on Server A)
Create a script `docker-backup.sh`:

```bash
#!/bin/bash
# Variables
CONTAINER_NAME=myapp
BACKUP_DIR=/opt/backups
TIMESTAMP=$(date +%F_%H-%M-%S)
BACKUP_FILE=$BACKUP_DIR/${CONTAINER_NAME}_$TIMESTAMP.tar.gz
REMOTE_USER=user
REMOTE_HOST=192.168.1.20    # Server B

# Create backup directory if not exists
mkdir -p $BACKUP_DIR

# Backup Docker container volume/data
docker run --rm --volumes-from $CONTAINER_NAME -v $BACKUP_DIR:/backup ubuntu \
    tar czf /backup/${CONTAINER_NAME}_$TIMESTAMP.tar.gz /data

# Copy backup to remote server
scp $BACKUP_FILE $REMOTE_USER@$REMOTE_HOST:/opt/restores/

# Delete backups older than 10 days
find $BACKUP_DIR -type f -name "*.tar.gz" -mtime +10 -exec rm -f {} \;
```
Make it executable:
```bash
chmod +x /opt/scripts/docker-backup.sh
```
### 📅 Cron Job Setup
### On Ubuntu/Debian

Edit root’s crontab:
```bash
sudo crontab -e
```

Add:
```bash
0 2 * * * /opt/scripts/docker-backup.sh >> /var/log/docker-backup.log 2>&1
```
👉 Runs every day at 2 AM.

### On CentOS/RHEL
```bash
sudo vi /etc/crontab
```

Add:
```bash
0 2 * * * root /opt/scripts/docker-backup.sh >> /var/log/docker-backup.log 2>&1
```

Reload cron service:
```bash
sudo systemctl restart crond
```
🛠️ Restore Script (on Server B)

To restore the latest backup:
```bash
#!/bin/bash
# Variables
RESTORE_DIR=/opt/restores
LATEST_BACKUP=$(ls -t $RESTORE_DIR/*.tar.gz | head -n 1)
CONTAINER_NAME=myapp-restore

# Create container and restore data
docker run --name $CONTAINER_NAME -v /data ubuntu bash -c "cd /data && tar xzf $LATEST_BACKUP --strip 1"
```
