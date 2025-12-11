# Containerization Booking Laravel Project

This project is a Laravel booking application. This README explains how to run it **locally**, with **Docker & Docker Compose**, and how to set up a **Jenkins pipeline**.

---

## 1. Running Locally (Without Docker)

### Prerequisites
- PHP >= 7.4
- Composer
- MySQL
- Node.js & NPM (for frontend assets)
- Git

### Steps

1. **Clone the repository**
```bash
git clone https://github.com/ahmedgamalyousef/Containerization-Booking-Laravel-Project.git
cd Containerization-Booking-Laravel-Project
```

2. **Install Composer dependencies**
```bash
composer install
```

3. **Copy .env file**
```bash
cp .env.example .env
```

4. **Set database credentials in `.env`**
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=booking
DB_USERNAME=bookinguser
DB_PASSWORD=Booking@1234
```

5. **Generate application key**
```bash
php artisan key:generate
```

6. **Run migrations & seed database**
```bash
php artisan migrate --seed
```

7. **Set permissions**
```bash
chmod -R 775 storage bootstrap/cache
```

8. **Run Laravel server**
```bash
php artisan serve
```
Access the app at: `http://127.0.0.1:8000`

---

## 2. Running With Docker & Docker Compose

### Prerequisites
- Docker
- Docker Compose

### Steps

1. **Clone the repository**
```bash
git clone https://github.com/ahmedgamalyousef/Containerization-Booking-Laravel-Project.git
cd Containerization-Booking-Laravel-Project
```

2. **Create `.env` file**
```bash
cp .env.example .env
```

3. **Update database credentials in `.env`**
```env
DB_HOST=db
DB_DATABASE=booking
DB_USERNAME=bookinguser
DB_PASSWORD=Booking@1234
```

4. **Build Docker images**
```bash
docker compose build
```

5. **Start containers**
```bash
docker compose up -d
```

6. **Wait for MySQL to be ready**
```bash
docker compose exec -T db mysqladmin ping -uroot -prootpass --silent
```

7. **Set permissions inside container**
```bash
docker compose exec -T app chown -R www-data:www-data /var/www/html
docker compose exec -T app chmod -R 775 /var/www/html/storage /var/www/html/bootstrap/cache
```

8. **Install dependencies & run migrations**
```bash
docker compose exec -T app composer install --no-interaction --prefer-dist
docker compose exec -T app php artisan key:generate --force
docker compose exec -T app php artisan migrate --force
docker compose exec -T app php artisan db:seed --force
docker compose exec -T app php artisan config:cache
```

9. **Access the app**
```
http://localhost:8000
```

---

## 3. Jenkins Pipeline Setup

### Prerequisites
- Jenkins installed
- Git plugin
- Docker installed on Jenkins node


### Notes:
- Make sure Jenkins has permissions to run Docker commands (`jenkins` user in `docker` group).
- Consider cleaning workspace before each build to avoid `.git/config.lock` issues.

---

## ✅ Summary
- Run locally: PHP + MySQL + Composer
- Run with Docker: Docker Compose up → setup `.env` → migrations
- Jenkins Pipeline: Automates Docker build, deployment, and Laravel setup