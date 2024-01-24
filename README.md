# Wordpress and XDebug Docker image

## Usage

Use the environment variable **XDEBUG_CONFIG** tu configure the XDebug PHP extension.

## Docker Compose

Example configuration file `docker-compose.yml`:

```yml
version: '3.3'

services:
  db:
    image: docker.io/bitnami/mariadb:10.3-debian-10
    restart: on-failure
    environment:
      MARIADB_USER: wordpress
      MARIADB_PASSWORD: wordpress
      MARIADB_ROOT_PASSWORD: wordpress
      MARIADB_DATABASE: wordpress

  wp:
    depends_on:
    - db
    image: insane/wordpress-xdebug
    volumes:
    - ./wp:/var/www/html
    ports:
    - 8080:80
    - 9000:9000
    restart: on-failure
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
```

## Usage with VSCode

To use XDebug in VSCode you need the [PHP Debug extension](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-debug).

You also need to make VSCode map the paths on the container to the ones on the host, you have to set the pathMappings settings in your `launch.json`.

Example configuration file `.vscode/launch.json`:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Listen for XDebug",
      "type": "php",
      "request": "launch",
      "port": 9000,
      "pathMappings": {
        "/var/www/html": "${workspaceRoot}/wp",
      }
    }
  ]
}
```

## Docker Hub

Published as [automattic/wordpress-xdebug](https://hub.docker.com/r/automattic/wordpress-xdebug) in **Docker Hub**.

## License

GPL v2

Based on previous work from [andreccosta/wordpress-xdebug](https://hub.docker.com/r/andreccosta/wordpress-xdebug)

