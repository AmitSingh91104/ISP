# Betternet - ISP Management System

## Project Overview

Betternet is a comprehensive Internet Service Provider (ISP) management system built with Laravel. It provides features for:

-   User management
-   Billing and invoicing
-   Package management
-   Router integration
-   Ticket support system
-   Payment processing

## Technology Stack

### Backend

-   PHP 8.3
-   Laravel 10
-   MariaDB/MySQL
-   RESTful API

### Frontend

-   Livewire
-   Tailwind CSS
-   Alpine.js
-   Vite

### Additional Components

-   Laravel Cashier (for subscriptions)
-   Laravel DomPDF (for PDF generation)
-   Bkash payment integration

## Installation

### Prerequisites

-   PHP 8.3 with extensions: dom, bcmath, xml, mysql
-   Composer 2.0+
-   Node.js 18+ and npm/yarn
-   MariaDB/MySQL 10.3+

### Setup Steps

1. Clone the repository:

```bash
git
cd betternet4
```

2. Install PHP dependencies:

```bash
composer install
```

3. Install JavaScript dependencies:

```bash
npm install
```

4. Create and configure .env file:

```bash
cp .env.example .env
```

5. Generate application key:

```bash
php artisan key:generate
```

6. Configure database settings in .env:

```ini
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=betternet4
DB_USERNAME=root
DB_PASSWORD=your_password
```

7. Create database:

```bash
sudo mysql -e "CREATE DATABASE betternet4; GRANT ALL PRIVILEGES ON betternet4.* TO 'root'@'localhost'; FLUSH PRIVILEGES;"
```

8. Run migrations and seed database:

```bash
php artisan migrate --seed
```

## Running Locally

1. Start the development server:

```bash
php artisan serve
```

2. Build frontend assets (in another terminal):

```bash
npm run dev
```

3. Access the application at:

```
http://localhost:8000
```

### Default Admin Credentials

-   Email: admin@betternet.com
-   Password: password

## Deployment to Production

### Server Requirements

-   Linux server (Ubuntu 22.04 recommended)
-   Nginx/Apache
-   PHP 8.3 with FPM
-   MySQL/MariaDB
-   Redis (for caching/sessions)
-   Supervisor (for queue workers)

### Deployment Steps

1. Set up production .env:

```ini
APP_ENV=production
APP_DEBUG=false
APP_URL=your_domain.com

# Configure database, cache, queue, etc.
```

2. Optimize application:

```bash
php artisan config:cache
php artisan route:cache
php artisan view:cache
```

3. Set up Nginx configuration (example):

```nginx
server {
    listen 80;
    server_name your_domain.com;
    root /var/www/betternet4/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    index index.php;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

4. Set up cron job for scheduled tasks:

```bash
* * * * * cd /path-to-your-project && php artisan schedule:run >> /dev/null 2>&1
```

## Troubleshooting

### Common Issues

1. **Database connection errors**:

    - Verify .env database credentials
    - Ensure MySQL service is running
    - Check user permissions

2. **Missing PHP extensions**:

    - Install required extensions: `sudo apt-get install php8.3-dom php8.3-bcmath php8.3-xml php8.3-mysql`

3. **Frontend assets not loading**:
    - Run `npm run build`
    - Check public/build directory permissions
