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

## How to publish a new build


1. Create a Pull Request bumping the version in `Dockerfile` for the `wordpress` image tag on [Automattic/wordpress-xdebug](https://github.com/Automattic/wordpress-xdebug) to get a new build done automatically. See [latest builds of automattic/wordpress-xdebug here](https://hub.docker.com/r/automattic/wordpress-xdebug/builds).
2. Merge the PR (if you're an Automattician)
3. Using GitHub's Interface, create a release pointing to master and with the release tag matching the version you justed bumped to in the Dockerfile. e.g. so it reads `FROM wordpress:5.6.1`.
	![image](https://user-images.githubusercontent.com/746152/94859157-7be94000-040a-11eb-8173-127d033b5238.png)
4. That's it, now `automattic/wordpress-xdebug:latest` and `automattic/wordpress-xdebug:5.6.1` will both refer to the new build based on the `wordpress:5.6.1` tag.

Based on previous work from [andreccosta/wordpress-xdebug](https://hub.docker.com/r/andreccosta/wordpress-xdebug.)

## License

GPL v2
