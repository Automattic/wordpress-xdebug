# Wordpress and XDebug Docker image

## Usage

Use the environment variable **XDEBUG_CONFIG** tu configure the XDebug PHP extension.

## Docker Compose

Example configuration file `docker-compose.yml`:

```yml
version: '3.3'

services:
  db:
    image: mysql:5.7
    restart: on-failure
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress

  wp:
    depends_on:
    - db
    image: automattic/wordpress-xdebug
    volumes:
    - ./wp:/var/www/html
    ports:
    - 8080:80
    restart: on-failure
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      XDEBUG_CONFIG: remote_host=host.docker.internal
```

## Usage with VSCode

To use XDebug in VSCode you need the [PHP Debug extension](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-debug).

You need to also make VSCode map the paths on the container to the ones on the host, you have to set the pathMappings settings in your `launch.json`.

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
