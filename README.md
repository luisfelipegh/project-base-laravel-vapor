# Laravel 8.x in Serverless

A guide to install and deployment laravel app with authentication in serverless 

First you need account in Vapor and AWS 

- **[Vapor](https://vehikl.com/)**
- **[AWS Iam](https://console.aws.amazon.com/iam)**

## Installation (Optional)

1. Installing dependencies

```bash
composer install
```

2. Create database project

``` sql
--example to mysql
CREATE DATABASE database_name DEFAULT CHARACTER SET UTF8;
CREATE USER your_username@'localhost' IDENTIFIED BY 'your_secret';
GRANT ALL PRIVILEGES ON database_name.* TO your_username@'localhost';
FLUSH PRIVILEGES;

```

3. Migrate and seed

```bash
php artisan migrate --seed
```

5. Generate app key

````bash
php artisan key:generate
````

## Deployment with vapor

In this section you need a vapor library installed in SO

```bash
composer require laravel/vapor-cli
```
or global installation 
```bash
composer global require laravel/vapor-cli
```
next you can validate installation with 
```bash
vendor/bin/vapor 
```
you can register vapor command (optional)
```
export PATH="$PATH:$HOME/.config/composer/vendor/bin"
```

you need modify or create a file vapor.yml with own preferences
```yaml
name: project-name
enviroments:
  production:
    memory: 1024
    cli-memory: 512
    runtime: php-7.4
    build:
      - 'composer install'
      - 'php artisan event:cache'
      - 'npm install && npm run prod && rm -rf node_modules'
  development:
    memory: 1024
    cli-memory: 512
    runtime: php-7.4
    build:
      - 'composer install'
      - 'php artisan event:cache'
      - 'npm install && npm run dev && rm -rf node_modules'
```

 
