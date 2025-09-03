# 📦 Migration Guide - Ambaturin Tecnologia

This guide explains how to **backup and restore** your WordPress (MySQL + uploads) and MongoDB data when moving between machines or resetting your Docker environment.

---

## 🔹 1. Backup

### a) Backup MySQL (WordPress DB)
```bash
# Export database from the running DB container
docker exec -i ambaturintechnologia-db-1   mysqldump -u wp_user -p wp_db > db_backup.sql
```
👉 Enter password: `wp_pass` (defined in `docker-compose.yml`).

This creates `db_backup.sql` on your host machine.

---

### b) Backup WordPress uploads, themes, plugins
Since WordPress files are bind-mounted, just copy them from your project folder:

```
/wordpress/wp-content/uploads/
/wordpress/wp-content/themes/
/wordpress/wp-content/plugins/
```

You can zip them for easier transfer:
```bash
tar -czvf wordpress_files_backup.tar.gz wordpress/wp-content
```

---

### c) Backup MongoDB
```bash
docker exec -i ambaturintechnologia-mongodb-1   mongodump --out /data/db/backup
```

Then copy the backup to your host:
```bash
docker cp ambaturintechnologia-mongodb-1:/data/db/backup ./mongo_backup
```

---

## 🔹 2. Restore

### a) Restore MySQL
```bash
# Copy the backup into the DB container
docker cp db_backup.sql ambaturintechnologia-db-1:/db_backup.sql

# Import it into the database
docker exec -i ambaturintechnologia-db-1   mysql -u wp_user -p wp_db < /db_backup.sql
```

---

### b) Restore WordPress uploads/themes/plugins
```bash
docker cp ./wordpress/wp-content/uploads ambaturintechnologia-wordpress-1:/var/www/html/wp-content/uploads
docker cp ./wordpress/wp-content/themes ambaturintechnologia-wordpress-1:/var/www/html/wp-content/themes
docker cp ./wordpress/wp-content/plugins ambaturintechnologia-wordpress-1:/var/www/html/wp-content/plugins
```

---

### c) Restore MongoDB
```bash
docker cp ./mongo_backup ambaturintechnologia-mongodb-1:/data/db/backup

docker exec -i ambaturintechnologia-mongodb-1   mongorestore /data/db/backup
```

---

## 🔹 3. Fresh start on new machine

1. Clone repo:
   ```bash
   git clone https://github.com/<your-username>/ambaturin-tecnologia.git
   cd ambaturin-tecnologia
   ```

2. Start services:
   ```bash
   docker-compose up -d --build
   ```

3. Restore your backups:
   * Import `db_backup.sql` into MySQL
   * Copy WordPress uploads/themes/plugins
   * Restore MongoDB dump

✅ You’re back to the same state as your old machine.
