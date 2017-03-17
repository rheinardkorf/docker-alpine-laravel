# docker-alpine-laravel
Basic Laravel setup with official Alpine Nginx docker image and official Alpine PHP-FPM docker image (extended with PDO MySQL drivers).

## Installation

### Docker

To use this project you first need Docker installed. It would do you well to learn more about Docker, but these steps will get
you going fast. The first step of course is to get Docker. You can get Docker from here:

* MacOS: https://www.docker.com/docker-mac
* Windows: https://www.docker.com/docker-windows
* Other: https://www.docker.com/get-docker

Once Docker is installed you should be able to run the following from command line (terminal / cmd.exe):  `docker -v`. Congrats
and welcome to the awesome world of Docker.

### Setting up your app

Setting up a Laravel app is pretty straight forward. Go to any of your project folders and then run the following (replacing my-app with whatever you wish):

```
git clone https://github.com/rheinardkorf/docker-alpine-laravel.git my-app
cd my-app
```

You need to rename a couple of files to get started. The easiest would be be to use the defaults and consider changing these later.

```
mv config/mysql.env.example config/mysql.env
mv src/.env.example srv/.env
```

Next you need to let Docker do its magic.

```
docker-compose up -d
```

Note: The first time you run `docker-compose` it might take a while to download and build the Docker images. It gets much faster after this!

This will spin up a few Docker containers:

* nginx - Which acts as a proxy for your site and the PHP FPM server.
* app - Your Laravel App. This is a PHP FPM image with PDO MySQL drivers installed.
* db - MySQL for serving your Laravel app a database.
* phpmyadmin - A web interface for your database.

## URLs

The following URLs will now be available for your project:

* http://localhost/ -- Your starter Laravel app.
* http://localhost:8080/ -- The PHPMyAdmin interface for your database (username: root, password: root)

You are now ready to build out your Laravel app.

## Project Folder Structure

Inside your primary `my-site` project folder you will see the following:

* docker-compose.yml -- The Docker configuration for your project.
* config/site.conf -- The nginx configuration for your site.
* config/mysql.env -- Configuration for your database.
* src/ -- The Laravel app and its whole application tree.
* logs/ -- If you poked in the docker-compose.yml file and enabled nginx logs, you will have this folder.
* docker/app -- A simple Dockerfile to add the PDO drivers to PHP-FPM.

## Stopping your project

Running the following will stop all active Docker containers: `docker stop $(docker ps -q)`

## Starting your project again

Make sure you are in your project folder (the one with docker-compose.yml) and run `docker-compose up -d`.

Simple.

## Using Artisan

If you love Laravel Artisan as much as I do, you might be wondering how to use it. Here are two options.

### Option 1:

Run it from your host. But first you will need your app's container name. Run the following and find your container's name in the far right column.

```
docker ps
```

It should be something like "app_app_<number>".

Now you can run Artisan using the following:

```
docker exec -ti app_app_1 php artisan
```
I recommend creating an alias if you plan to use it a lot.

### Option 2:
Drop into your container's shell (its Kornshell, so don't expect all the wow of zsh, fish or even basic ol' bash). To do this run the following:

```
docker exec -ti app_app_1 /bin/sh
```

## TIP:
The `src` folder is a standard Laravel app. Replace the contents with your existing Laravel app to dockerize it.
*NOTE*: You will need to change your .env file to use `dbhost` as your database host to use the MySQL container. Using localhost will fail.

Enjoy.
