# PHP-Websites-using-Docker-Containers-with-PHP-Apache-and-MySQL

* Setup and run a local PHP Apache server instance.
* Serve a dynamic PHP-driven website.
* Setup a MySQL database to run SQL scripts, fetch records, and print them in a PHP-driven website.

 ## Step 1.Create docker-compose YML file
 
 ```
 version: '3.8'
services:
    php-apache-environment:
        container_name: php-apache
        build:
            context: ./php
            dockerfile: Dockerfile
        depends_on:
            - db
        volumes:
            - ./php/src:/var/www/html/
        ports:
            - 8000:80
    db:
        container_name: db
        image: mysql
        restart: always
        environment:
            MYSQL_ROOT_PASSWORD: MYSQL_ROOT_PASSWORD
            MYSQL_DATABASE: MYSQL_DATABASE
            MYSQL_USER: MYSQL_USER
            MYSQL_PASSWORD: MYSQL_PASSWORD
        ports:
            - "9906:3306"
 
 ```
 
 * The container name - this is just a random name that you would like to name your PHP container.
 * The container image - this the official PHP image, the version of PHP Apache you want to use.
 * The volume - this will set up your present working src directory for your code/source files. If you were to run a PHP script, that file would have to be in that directory.
 * The port numbers. This defines the ports where the script will run from. 

## Serve a dynamic PHP-driven website

You will be executing the scripts from the directory that you defined in the volumes of your docker-compose.

In this case we are using ./php/src.

Head on to your project directory ➙ ./php/src, create an index.php file and start writing your PHP scripts.

A simple index.php script.

```
<?php
echo "Hello there, this is a PHP Apache container";
?>

```

