# Software Update & Maintenance Guide

This guide provides best practices for developers, system administrators, and site owners when performing updates, adding new features, or modifying the AnimeBro codebase.

---

## 1. Development Principles & Code Architecture

AnimeBro is written in clean, modular, modern PHP (7.4 through 8.3+ compatible) with PDO database abstraction:

- **Central Configuration**: All environment configurations and global helpers are centralized in `common/config.php` and `common/db_config.php`.
- **Self-Healing Database Architecture**: The platform includes automatic schema checking in initialization routines, ensuring newly added columns and tables are created seamlessly.
- **Role & Permission Guard**: Built-in permission checks (`require_admin_auth()`, `require_permission()`) protect admin routes.
- **CSRF Token Validation**: All state-changing POST requests require `csrf_field()` and `require_csrf()`.

---

## 2. Safe Update Workflow (Staging to Production)

Never test code modifications directly on a live production server. Follow this standard 4-step workflow:

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│ 1. Local Backup │ ──> │ 2. Staging Test │ ──> │ 3. Code Deploy  │ ──> │ 4. Verification │
└─────────────────┘     └─────────────────┘     └─────────────────┘     └─────────────────┘
```

### Step 1: Create a Pre-Update Snapshot
Always create a full database backup and zip of your files before deploying updates. See [BACKUP-RESTORE-GUIDE.md](BACKUP-RESTORE-GUIDE.md).

### Step 2: Test on Staging / Subdomain
1. Clone the production site to a staging subdomain (e.g., `staging.yourdomain.com`).
2. Apply code modifications, bug fixes, or new templates.
3. Test all core functionalities:
   - User authentication & profile settings
   - Video upload engine & player servers
   - TVDB scraping and import
   - Admin settings update & Landing page editor

### Step 3: Incremental Code Deployment
1. Transfer modified PHP/CSS/JS files to production.
2. If new database columns or tables are introduced, execute the SQL migration script via phpMyAdmin or MySQL CLI.
3. Ensure file permissions remain intact (`chmod 644` for files, `chmod 755` for folders, `chmod 775` for `uploads/`).

### Step 4: Post-Deployment Verification
1. Clear browser cache and service worker caches.
2. Perform a test watch session on `/home` and `/watch/*`.
3. Check the PHP error log for any warnings or notices.

---

## 3. Database Schema Migrations Guide

When adding new features that require database schema adjustments:

1. **Use `IF NOT EXISTS`**:
   ```sql
   CREATE TABLE IF NOT EXISTS my_new_feature (
       id INT AUTO_INCREMENT PRIMARY KEY,
       name VARCHAR(255) NOT NULL,
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
   ```
2. **Safe Column Alterations**:
   When adding columns to existing tables, verify if the column already exists in your migration logic or check `SHOW COLUMNS FROM table_name LIKE 'column_name'`.

---

## 4. Upgrading PHP Versions

AnimeBro is tested and optimized for PHP 7.4, 8.0, 8.1, 8.2, and 8.3+.

When upgrading your server PHP version:
1. Ensure all required extensions are enabled in the new PHP runtime:
   - `pdo_mysql`
   - `curl`
   - `mbstring`
   - `json`
   - `gd`
   - `session`
2. Update `php.ini` memory and upload limits (`upload_max_filesize = 2048M`, `post_max_size = 2048M`, `memory_limit = 512M`).
3. Restart web server / PHP-FPM.
