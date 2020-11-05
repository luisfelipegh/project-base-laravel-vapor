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
composer require laravel/vapor-core
```
or global installation 
```bash
composer global require laravel/vapor-cli
composer global require laravel/vapor-core
```
next you can validate installation with 
```bash
vendor/bin/vapor 
```
you can register vapor command (optional)
```
export PATH="$PATH:$HOME/.config/composer/vendor/bin"
```

## Yaml vapor File

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

additional params in environments object 
```yaml

some-environment:
    storage: your-name-storage
    deploy: 
        - 'some script'
        - 'other script'
    database: your-database-name
    domain: your-domain
    cache: your-server-cache
    mail: 
    scheduler:
    queues: 
    queue-timeout:
    queue-concurrency:
```

### Optionals implementations params

if you need upload files in storage, you will create a policy 
```bash
php artisan make:policy UserPolicy --model=User
```
next, you need add method to upload file 

``` php
/**
 * Determine whether the user can upload files. 
 *
 * @param  \App\User  $user
 * @return mixed
 */
public function uploadFiles(User $user)
{
    return true;
}
```
## Executing

now, you need execute the following command and configure team, project and login to vapor account
```bash
vapor login
```

finally, you can start deploy your project with preference environment configured in vapor.yml

```bash
vapor deploy <environment>
``` 


