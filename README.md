# Demo of how to use docker secrets (in Docker Swarm) as environment variables.
## Usage
This repo contains an example of how to expose the `my_password_secrets` docker secret as an environment variable. This is named throughout the repo as 
```
my_password_secrets
MY_PASSWORD_SECRETs
MY_PASSWORD_SECRET_FILE
```
but anything would work. The example is inspired by how this is implemented in the official MySQL and WordPress images:
- https://github.com/docker-library/mysql/blob/master/5.7/docker-entrypoint.sh
- https://github.com/docker-library/wordpress/blob/master/docker-entrypoint.sh

## Initialize Swarm and create a secret
```bash
docker swarm init
echo "mysupersecurepassword" | docker secret create my_password_secrets
```

## Clone repo and deploy stack
```bash
git clone https://github.com/adrian-gheorghe/demo-docker-secrets-env-vars.git demo-docker-secrets-env-vars
cd demo-docker-secrets-env-vars
```
Build images first (For this demo, the images will not be loaded from an external Docker registry) but build from the repo directly
```
docker-compose build
```

Deploy stack and check logs
```bash
docker stack deploy -c docker-compose.yaml secrets --prune
docker service logs secrets_php -f
```

Output should be something along the following lines
```bash
secrets_php.1.odzanf4ar9dw@docker-desktop    | string(21) "mysupersecurepassword"
```
This is because the php container command is set to only run the index.php file included in the folder.

This file contains the following:
```php
<?php

var_dump($_ENV['MY_PASSWORD_SECRET']);
```