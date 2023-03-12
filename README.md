# Laravel Docker 🐳

<p align="center">
<img src="https://repository-images.githubusercontent.com/604504498/60f54e29-db30-401a-9d2f-d14864cd5be0">
</p>
A generic docker environment for Laravel with mySql and Nginx


## Insstallation

1. Click [Use this template](https://github.com/rakibdevs/laravel-docker/generate)
2. Clone the repository containing the `docker-compose.yml` file.
3. Run the following command to build the Docker image:

```bash
$ docker compose build
```
This command will download all the necessary dependencies and build the Docker image according to the specifications in the Dockerfile.

4. Once the build is complete, run the following command to start the Docker container: 
```bas
$ docker-compose up -d
```
5. Run the following command to create a new Laravel project inside a src directory:
```bash
$ sudo chmod 777 src/
$ docker compose exec php composer create-project --prefer-dist laravel/laravel .
```
This command will install all the necessary dependencies and create a new Laravel project inside the src directory.

6. Run other esential commands -
```bash
$ docker compose exec php php artisan storage:link
$ docker compose exec php chmod -R 777 storage bootstrap/cache
```
7. Run the following command to migrate database `docker compose exec php php artisan migrate:refresh`. Before migration please update environment variable for database.
```DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=1234
```

## Acces Webpage
To access the web page for your Laravel project, please follow these steps:

Ensure that the Docker container is running by running the command docker ps in your terminal or command prompt. This command will display a list of all the running Docker containers on your system.

Open a web browser and navigate to the following URL: http://localhost:8999. This URL corresponds to the port that has been exposed in the `docker-compose.yml` file for the Nginx service.

## How to setup different PHP version?
To update the PHP version for your Docker container, please follow these steps:

Open the `Dockerfile` for your PHP image, which is located in the `docker/php/` directory.

Update the FROM statement to use the base image for the PHP version you want to use. For example, to use PHP version 7.4, you can change the FROM statement to the following:

```bash
FROM php:7.4-fpm
```
Save the Dockerfile and build the Docker image again using the `docker-compose build` command.

Once the build is complete, start the Docker container using the `docker-compose up` command.
