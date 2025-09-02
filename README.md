# 🛠️ Ambaturin Tecnologia - 3D Printing Shop Project

This project combines **WordPress + WooCommerce** for the online store with a **custom Next.js dashboard** for monitoring 3D printing manufacturing progress. All services run in Docker containers, including **MongoDB** for tracking production data.

---

## 🚀 Prerequisites

* [Docker](https://www.docker.com/get-started)
* [Docker Compose](https://docs.docker.com/compose/install/)
* Git

---

## 🟢 1. Clone the repository

```bash
git clone https://github.com/<your-username>/ambaturin-tecnologia.git
cd ambaturin-tecnologia
```

---

## 🟢 2. Start Docker services

```bash
docker-compose up -d --build
```

This will start:

* **WordPress** → [http://localhost:8080](http://localhost:8080)
* **phpMyAdmin** → [http://localhost:8081](http://localhost:8081)
* **Next.js Dashboard** → [http://localhost:3000](http://localhost:3000)
* **MongoDB** → `mongodb://localhost:27017`

---

## 🟢 3. Set up WordPress / WooCommerce

1. Open [http://localhost:8080](http://localhost:8080)
2. Complete WordPress installation
3. Install **WooCommerce plugin** to enable your e-commerce store
4. Add products as needed

> **Note:** WordPress DB is inside the Docker container (`db`). If you already have a `db_dump.sql`, see the import instructions below.

---

## 🟢 4. Optional: Import existing WooCommerce database

If you have an existing database dump (`db_dump.sql`):

```bash
# Copy the dump into the DB container
docker cp db_dump.sql ambaturintechnologia-db-1:/db_dump.sql

# Import the dump
docker exec -i ambaturintechnologia-db-1 \
  mysql -u wp_user -p wp_db < /db_dump.sql
```

Password for `wp_user` is defined in `docker-compose.yml` (`wp_pass`).

---

## 🟢 5. Access the Next.js Dashboard

Your **custom dashboard** runs at:

```
http://localhost:3000
```

* Modify `/dashboard/pages` and `/dashboard/components` for your own features
* Dashboard connects to **MongoDB** for manufacturing monitoring

---

## 🟢 6. Stopping the project

```bash
docker-compose down
```

> To remove all volumes (DB + WordPress data) for a fresh start:

```bash
docker-compose down -v
```

---

## ✅ Notes

* Do **not commit Docker volumes or WordPress uploads** to GitHub. `.gitignore` handles this.
* Next.js hot-reloading works automatically when editing `/dashboard` files.
* Use `docker logs -f <container>` to debug services if needed.
