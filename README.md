# Booking System with Laravel 8 and Material Dashboard

## How to run this project locally : 

- Clone the repository with __git clone__
- Copy __.env.example__ file to __.env__
- Edit database credentials in __.env__
- Run __composer install__
- Run __php artisan key:generate__
- Run __php artisan migrate --seed__
- Run __npm install__
- Run __npm run dev__
- Run __php artisan serve__ (if you want to use other port add __--port=90__)
- You can __register__ by clicking on top-right


## How to run this project inside containers using Docker & Docker-Compose :

A simple booking application built with **Laravel 8**, **MySQL**, **Docker**, and **Nginx**.

## Table of Contents
- [Project Setup](#project-setup)
- [Requirements](#requirements)
- [Installation](#installation)
- [Running the Project](#running-the-project)
- [Database Setup](#database-setup)
- [Seeding](#seeding)
- [Usage](#usage)
- [Troubleshooting](#troubleshooting)
- [License](#license)



## Project Setup

This project uses Docker to run Laravel, MySQL, and Nginx. It also supports environment configuration and database seeding .


## Requirements

- Docker   
- Docker Compose  
- Git
- Nginx
- MYSQL
  


## Installation

1. Clone the repository:

```bash
git clone https://github.com/ahmedgamalyousef/Dockerizing-Booking-Laravel-Project.git
cd Dockerizing-Booking-Laravel-Project
```

2. Copy the environment file:

```bash
cp .env.example .env
```

3. Build Docker images:

```bash
docker compose build
```

---

## Running the Project

Start all services:

```bash
docker compose up -d
```

Check containers:

```bash
docker compose ps
```

Your app should be accessible at:  
[http://localhost:8000](http://localhost:8000)

---

## Database Setup

1. Run migrations:

```bash
docker compose exec app php artisan migrate --force
```

2. Seed the database:

```bash
docker compose exec app php artisan db:seed --force
```

3. Generate Laravel application key:

```bash
docker compose exec app php artisan key:generate --force
```

---

## Usage

- Register a new user.  
- Access posts at `/posts`.  
- Access admin panel if roles are configured.

---

## Troubleshooting

**Permission Errors** (e.g., `file_put_contents(): failed to open stream`)  

Run inside the container:

```bash
docker compose exec app bash
chown -R www-data:www-data /var/www/html/storage /var/www/html/bootstrap/cache
chmod -R 775 /var/www/html/storage /var/www/html/bootstrap/cache
php artisan cache:clear
php artisan config:clear
php artisan route:clear
php artisan view:clear
```

**Database connection issues**  

- Ensure MySQL container is running.
- Check `.env` database credentials.
# Containerization-Booking-Laravel-Project
# Containerization-Booking-Laravel-Project
