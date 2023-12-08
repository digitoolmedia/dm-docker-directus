# Directus deploy

Fill your env variables, and `compose up` the version you need.

If you pull this git but also wish to create your own deployment you could simply use the following file naming convention: `docker-compose.SOMETHING.yml`.

### Notes
- traefik deployment currently not maintained for lack of testing environment, but open to PRs.

## Generating the UNIQUE_KEY and UNIQUE_SECRET

A good and practical way to generate them would be to use:    
`openssl rand -hex 16` (output example: `ed84e94edee1a90bb82c1503fca9ce3e`)    
`openssl rand -base64 24` (output example: `NR0OLD1KXuZQu2aZyiGTzdmIEVy0IoLp`)

## Directus config reference:

https://docs.directus.io/self-hosted/config-options#general


# Commands

To execute any command from outside the container prepend them with the following code, making sure to specify the correct container name:

``` bash
docker exec -t YOUR-CONTAINER-NAME 
```

## backup database

``` bash
mkdir -p /db_dumps && pg_dump "${DB_NAME}" -c -U "${DB_USER}" | gzip > /db_dumps/$(date +"%Y-%m-%d_%H-%M-%S")_"${DB_NAME}".sql.gz
```

## restore database

1. Stop Directus and cache containers/services.

2. Execute the following command:
``` bash
psql template1 -c "drop database \"${DB_NAME}\" with (force);" -U "${DB_USER}" && psql template1 -c "create database \"${DB_NAME}\" with owner "${DB_USER}";" -U "${DB_USER}" && gunzip -c $(ls -1t /db_dumps/*.sql.gz | head -n1) | psql "${DB_NAME}" -U "${DB_USER}"
```

3. 
After successful restore start Directus and cache containers/services.

## delete Directus transformations

attach to Directus's running container and run the following comand:

``` bash
find /directus/uploads -type f -mtime +0 -regex "^\([0-9a-z]\{8\}\)-\([0-9a-z]\{4\}-\)\{3\}\(\([0-9a-z]\{12\}\)\)\(__[0-9a-z]\{40\}\)\.\(jpeg\|jpg\|webp\|avif\|svg\|png\)$" -delete
```

In production you could also execute this command from `filebrowser`s terminal.

Ideally this should be added to a cron-job, but make sure to change `-mtime` from `+0` to `+29`, to match Directus's `maxAge`