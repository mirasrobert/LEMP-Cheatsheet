# Deploying Laravel on DigitalOcean with LEMP Stack

This cheatsheet provides step-by-step instructions to deploy and set up a Laravel application on a DigitalOcean Ubuntu droplet with a LEMP stack.

## Prerequisites

- [x] DigitalOcean account with a created droplet (Ubuntu 20.04 recommended).
- [x] Basic server administration skills.
- [x] Git installed on your local machine.
- [x] A Laravel application hosted on GitHub (e.g., "laravel_example_app").

## Initial Server Setup

1. **Create a DigitalOcean Droplet:**
   - Choose Ubuntu 20.04 as the operating system.
   - Follow the DigitalOcean wizard to create the droplet.

2. **Access Your Droplet:**
   - Open your terminal and use SSH to access your droplet:
     ```bash
     ssh root@your_droplet_ip
     ```

3. **Update Server Packages:**
   ```bash
   sudo apt update
   sudo apt upgrade -y # optional
   
4. **Install NGINX Web Server**
    ```bash
    sudo apt install nginx
    
    # Start NGINX and Enable it.
    systemctl start nginx
    systemctl enable nginx   
   
5. **Install MySQL:**
    ```bash
    sudo apt install mysql-server
    sudo mysql_secure_installation # modify some insecure defaults

6. **Change root mysql password:**
    ```bash
    sudo mysql
    ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your_password';
    mysql -u root -p # Access mysql
    
7. **Install PHP and Required Extensions:**
    ```bash
    sudo apt install php php-fpm php-mysql php-common php-bcmath php-ctype php-json php-mbstring php-openssl php-pdo php-tokenizer php-xml php-zip php-gd
    
8. **Configure Nginx for Laravel:**
    - Create an Nginx server block configuration for your Laravel app (e.g., /etc/nginx/sites-available/laravel_example_app).
    - Configure the Nginx server block to use PHP-FPM for processing PHP files.
    
9. **Create server block config**
    ```bash
    sudo nano /etc/nginx/sites-available/laravel_example_app
    ```
    
    ```bash
    # NGINX CONFIG [https://laravel.com/docs/7.x/deployment]
    server {
    listen 80;
    server_name your_droplet_ip;
    root /var/www/laravel_example_app/public;
 
    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";
 
    index index.php;
 
    charset utf-8;
 
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
 
    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }
 
    error_page 404 /index.php;
 
    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }
 
    location ~ /\.(?!well-known).* {
        deny all;
    }
}
    ```
    
10. **Enable the Nginx Configuration:**
    - Create a symbolic link to enable the site:
    ```bash
    sudo ln -s /etc/nginx/sites-available/laravel_example_app /etc/nginx/sites-enabled/
    ```

    - Unlink the default conf
    ```bash
    sudo unlink /etc/nginx/sites-enabled/default


11. **Test Nginx Configuration:**
    ```bash
    nginx -t

## Deploy Laravel Application

1. **Clone Your Laravel App from GitHub:**
    ```bash
    git clone https://github.com/yourusername/laravel_example_app.git /var/www/laravel_example_app

2. **Install Composer**    
    - https://getcomposer.org/download/

3. **Install Composer Dependencies**
    ```bash
    cd /var/www/laravel_example_app
   composer install --no-interaction --prefer-dist

4. **Generate Laravel Application Key:**
    ```bash
    php artisan key:generate

5. **Set Permissions:**
    - Ensure proper file permissions for Laravel Application
    ```bash
    chown -R www-data:www-data /var/www/laravel_example_app
    chmod -R 755 /var/www/laravel_example_app/storage


6. **Access MySQL:**
    ```bash
    mysql -u root -p

7. **Create a database and user:**
    ```bash
    CREATE DATABASE laravel_example_app_db;
    CREATE USER 'laravel_user'@'localhost' IDENTIFIED BY 'your_password';
    GRANT ALL PRIVILEGES ON laravel_example_app_db.* TO 'laravel_user'@'localhost';
    FLUSH PRIVILEGES;

8. **EXIT MYSQL**
    ```bash
    exit

9. **Configure .env**
    - Update the Database Configuration
    ```bash
    cd /var/www/laravel_example_app
    nano .env

    
10. **Migrate and Seed Database (Laravel):**
    ```bash
    php artisan migrate --seed

## Final Steps
1. **Reload NGINX**
    ```bash
    systemctl reload nginx

2. **Restart PHP-FPM**
    ```bash
    systemctl restart php7.4-fpm
