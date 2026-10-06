# Backup & Disaster Recovery Guide

This guide details best practices and step-by-step procedures for backing up, maintaining, and restoring the AnimeBro website files and MySQL database.

---

## 1. What Needs to Be Backed Up

A complete AnimeBro backup consists of three essential components:

1. **MySQL Database**: All tables (`anime`, `episodes`, `users`, `settings`, `comments`, `landing_*`, etc.).
2. **Uploaded Media Files**: The `/uploads/` directory containing site logos, hero backdrops, user avatars, and custom thumbnails.
3. **Application Source Code & Configuration**: All PHP files, assets, `.htaccess`, and `common/db_config.php`.

---

## 2. Manual Backup Procedures

### A. Database Backup via phpMyAdmin
1. Log into your hosting control panel and open **phpMyAdmin**.
2. Select your AnimeBro database from the left navigation bar.
3. Click the **Export** tab at the top.
4. Choose **Quick - display only the minimal options** (or **Custom** with `Add DROP TABLE / VIEW / PROCEDURE / FUNCTION / EVENT` enabled).
5. Format: **SQL**.
6. Click **Export** to download `your_database_name.sql`.

### B. Database Backup via SSH / CLI (mysqldump)
Run the following command on your server:
```bash
mysqldump -u animebro_user -p --single-transaction --routines --triggers animebro_db > animebro_backup_$(date +%F_%H%M%S).sql
```

### C. File System Backup via cPanel File Manager
1. Open **cPanel -> File Manager**.
2. Navigate to `public_html` (or your root installation folder).
3. Select all files and folders.
4. Click **Compress** (Choose `.zip` or `.tar.gz`).
5. Download the resulting archive to your secure storage.

### D. File System Backup via SSH / CLI
```bash
tar -czvf animebro_files_backup_$(date +%F).tar.gz --exclude="*.log" --exclude="*.tar.gz" -C /var/www/html/animebro .
```

---

## 3. Automated Daily Backup Script (Cron Job)

You can set up an automated daily backup script on your Linux VPS / cPanel cron:

Create a script `/root/scripts/backup_animebro.sh`:
```bash
#!/bin/bash
BACKUP_DIR="/root/backups/animebro"
DATE=$(date +%Y-%m-%d_%H%M%S)
mkdir -p "$BACKUP_DIR"

# 1. Backup Database
mysqldump -u animebro_user -p'YOUR_STRONG_PASSWORD' animebro_db | gzip > "$BACKUP_DIR/db_$DATE.sql.gz"

# 2. Backup Uploads Directory
tar -czf "$BACKUP_DIR/uploads_$DATE.tar.gz" /var/www/html/animebro/uploads/

# 3. Retain only backups from last 14 days
find "$BACKUP_DIR" -type f -mtime +14 -name "*.gz" -delete
```

Make it executable and add to crontab:
```bash
chmod +x /root/scripts/backup_animebro.sh
crontab -e
# Run daily at 3:00 AM:
0 3 * * * /root/scripts/backup_animebro.sh >/dev/null 2>&1
```

---

## 4. Disaster Recovery & Restoration Procedure

If your server crashes or you need to restore your website to a fresh server:

### Step 1: Restore Web Files
1. Extract your files archive into the web root (`public_html` or `/var/www/html`).
2. Verify permissions:
   ```bash
   chown -R www-data:www-data /var/www/html
   find /var/www/html -type d -exec chmod 755 {} \;
   find /var/www/html -type f -exec chmod 644 {} \;
   chmod -R 775 /var/www/html/uploads
   ```

### Step 2: Restore Database
1. Create a fresh MySQL database and user.
2. Import the backup SQL dump:
   ```bash
   mysql -u animebro_user -p animebro_db < animebro_backup.sql
   # Or if gzipped:
   gunzip < db_2026-10-06.sql.gz | mysql -u animebro_user -p animebro_db
   ```

### Step 3: Update Database Configuration
1. Edit `common/db_config.php` and fill in the new database host, name, user, and password.
2. Open your website in a browser and test frontend streaming and admin panel access.
