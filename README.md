# Larock

**Warning: I'm working on this so we're talking pre-alpha**

An entirely opinionated personal Laravel Docker [Compose]
setup for local development using PHPStorm (well ... IntelliJ
with the PHPStorm plugins).

Larock is a docker compose scaffold.

Maybe this will evolve into something more at some point.


## Usage

Generate the scaffold
```
% npm install -g degit
% degit jobyh/larock your-laravel-project
% cd your-laravel-project
```

Create your `.env` file
```
% cp .env.dist .env
```

Update the environment variables as necessary. The serverName
variable in PHP_IDE_SETTINGS should match the name of the server
configuration you have set up in PHPStorm.

XDEBUG_REMOTE_HOST should match the IP address of your machine
**on the network which Docker sets up**. You'll need to build
and run by continuing to follow the instructions then using
`ip address` or similar to get the address.


Add your Laravel project
```
# new project (directory 'html' matches .env CODE_ROOT)
% laravel new html

# existing project
% git clone <git_url> html

```

Build the containers

```
docker build
```

Run Docker
```
docker-compose up -d
```


