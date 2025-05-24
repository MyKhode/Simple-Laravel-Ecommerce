### Simple Laravel Ecommerce – Docker Setup Guide
This guide explains how to set up, run, and redeploy the Laravel 7.x Ecommerce project using Docker with MySQL 5.7 and PHP 7.4.

##### 🚀 How to Start the Project
- Clone or copy the project to any machine.
- run locally - not only for docker
```
php artisan migrate
php artisan db:seed
php artisan key:generate
php artisan serve
```

- In the project directory, run: - this for docker

```
docker compose build 
```
```
docker compose up -d
```
- On first run, enter the container and set up Laravel:
```
docker exec -it simple-laravel-online-shop-app-1 bash
php artisan migrate
```
##### 🧹 Cleanup and Rebuild
```
docker-compose down
```
```
docker-compose up --build
```

```
docker image prune -a
```

```
docker builder prune -a
```

##### 🔁 Ready to Redeploy?

Just copy the project to a new machine and run:

```
docker-compose up --build
```
Then inside the container:
```
php artisan migrate
php artisan session:table
php artisan migrate
```


##### 📦 Requirements

    Docker

    Docker Compose

    Optional: MySQL Client or phpMyAdmin for database access

##### 📁 Project Structure
Ensure your project folder contains the following:

```
.
├── app/               # Laravel app directory
├── .env               # Environment config (see setup below)
├── Dockerfile         # PHP 7.4 Dockerfile
├── docker-compose.yml # Services: app + MySQL

```

##### ⚙️ Environment Variables (.env)
Create or edit .env file:
```
APP_NAME=Laravel
APP_ENV=local
APP_KEY=base64:generated_key
APP_DEBUG=true
APP_URL=http://localhost:8000

LOG_CHANNEL=stack

DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=simple_shop
DB_USERNAME=root
DB_PASSWORD=root

SESSION_DRIVER=database

```

##### 🐋 Docker Compose Setup (docker-compose.yml)
```
version: '3.8'

services:
  app:
    build: .
    volumes:
      - .:/app
    working_dir: /app
    ports:
      - "8000:8000"
    depends_on:
      - mysql
    environment:
      DB_CONNECTION: mysql
      DB_HOST: mysql
      DB_PORT: 3306
      DB_DATABASE: simple_shop
      DB_USERNAME: root
      DB_PASSWORD: root

  mysql:
    image: mysql:5.7
    restart: always
    ports:
      - "3307:3306"  # Use 3307 if 3306 is occupied
    environment:
      MYSQL_DATABASE: simple_shop
      MYSQL_ROOT_PASSWORD: root
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:

```

##### 🛠 Dockerfile (PHP 7.4 + Composer)

```
FROM php:7.4-cli

RUN apt-get update && apt-get install -y \
    unzip git zip curl libpng-dev libonig-dev libxml2-dev libzip-dev \
    && docker-php-ext-install pdo_mysql mbstring zip exif pcntl

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /app

```
