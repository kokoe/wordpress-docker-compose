# WPDC - WordPress Docker Compose

Easy WordPress development with Docker and Docker Compose.

With this project you can quickly run the following:

- [WordPress and WP CLI](https://hub.docker.com/_/wordpress/)
- [phpMyAdmin](https://hub.docker.com/r/phpmyadmin/phpmyadmin/)
- [MySQL](https://hub.docker.com/_/mysql/)

Contents:

- [Requirements](#requirements)
- [Configuration](#configuration)
- [Installation](#installation)
- [Usage](#usage)

## Requirements

Make sure you have the latest versions of **Docker** and **Docker Compose** installed on your machine.

Clone this repository or copy the files from this repository into a new folder. In the **docker-compose.yml** file you may change the IP address (in case you run multiple containers) or the database from MySQL to MariaDB.

Make sure to [add your user to the `docker` group](https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user) when using Linux.

## Configuration

Edit the `.env` file to change the default IP address, MySQL root password and WordPress database name.

## Installation

Install the app on your local machine

- [Node.js](https://nodejs.org/)

Open a terminal and `cd` to the folder in which `docker-compose.yml` is saved and run:

```bash
docker-compose up

# npm
npm start
```

This creates two new folders next to your `docker-compose.yml` file.

- `wp-data` – used to store and restore database dumps
- `wp-app` – the location of your WordPress application

The containers are now built and running. You should be able to access the WordPress installation with the configured IP in the browser address. By default it is `http://127.0.0.1`.

For convenience you may add a new entry into your hosts file.

## Usage

### Starting containers

You can start the containers with the `up` command in daemon mode (by adding `-d` as an argument) or by using the `start` command:

```bash
docker-compose start

# npm
npm start
```

### Stopping containers

```bash
docker-compose stop

# npm
npm run stop
```

### Removing containers

To stop and remove all the containers use the`down` command:

```bash
docker-compose down

# npm
npm run down
```

Use `-v` if you need to remove the database volume which is used to persist the database:

```bash
docker-compose down -v

# npm
npm run down:db
```

For other commands, see npm scripts in [package.json](./package.json)

### Project from existing source

Copy the `docker-compose.yml` file into a new directory. In the directory you create two folders:

- `wp-data` – here you add the database dump
- `wp-app` – here you copy your existing WordPress code

You can now use the `up` command:

```bash
docker-compose up

# npm
npm run up
```

This will create the containers and populate the database with the given dump. You may set your host entry and change it in the database, or you simply overwrite it in `wp-config.php` by adding:

```php
define('WP_HOME','http://wp-app.local');
define('WP_SITEURL','http://wp-app.local');
```

### Creating database dumps

```bash
./export.sh
```

### Developing a Theme

Configure the volume to load the theme in the container in the `docker-compose.yml`:

```yaml
volumes:
  - ./theme-name/trunk/:/var/www/html/wp-content/themes/theme-name
```

### Developing a Plugin

Configure the volume to load the plugin in the container in the `docker-compose.yml`:

```yaml
volumes:
  - ./plugin-name/trunk/:/var/www/html/wp-content/plugins/plugin-name
```

### WP CLI

The docker compose configuration also provides a service for using the [WordPress CLI](https://developer.wordpress.org/cli/commands/).

Sample command to install WordPress:

```bash
docker-compose run --rm wpcli core install --url=http://localhost --title=test --admin_user=admin --admin_email=test@example.com
```

Or to list installed plugins:

```bash
docker-compose run --rm wpcli plugin list

# npm
npm run wp plugin list
```

For an easier usage you may consider adding an alias for the CLI:

```bash
alias wp="docker-compose run --rm wpcli"
```

This way you can use the CLI command above as follows:

```bash
wp plugin list
```

### phpMyAdmin

You can also visit `http://127.0.0.1:8080` to access phpMyAdmin after starting the containers.

The default username is `root`, and the password is the same as supplied in the `.env` file.
