# PHP-Websites-using-Docker-Containers-with-PHP-Apache-and-MySQL

* Setup and run a local PHP Apache server instance.
* Serve a dynamic PHP-driven website.
* Setup a MySQL database to run SQL scripts, fetch records, and print them in a PHP-driven website.

 ## Step 1.Create docker-compose YML file
 


We need to add some MySQL support tools inside the PHP container for the two services (db and php-apache) to work correctly. This tool includes mysqli.

Inside your project directory, head to the /php folder, create a Docker file, name it Dockerfile and add the following PHP configurations.

```
FROM php:8.0-apache
RUN docker-php-ext-install mysqli && docker-php-ext-enable mysqli
RUN apt-get update && apt-get upgrade -y

```

Now we need to build this custom image inside php-apache service in the docker-compose.yml file. PHP Apache also depends on the db service to connect to MySQL. We need to configure it by specifying a depends_on: environment.

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

## Run SQL query using PHP scripts

Let’s test if the container is working as expected. Head over to the index.php file and the following PHP MySQL connection code.

```
<?php
//These are the defined authentication environment in the db service

// The MySQL service named in the docker-compose.yml.
$host = 'db';

// Database use name
$user = 'MYSQL_USER';

//database user password
$pass = 'MYSQL_PASSWORD';

// check the MySQL connection status
$conn = new mysqli($host, $user, $pass);
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
} else {
    echo "Connected to MySQL server successfully!";
}
?>

```


Save the file and refresh your http://localhost:8000/ web address.


