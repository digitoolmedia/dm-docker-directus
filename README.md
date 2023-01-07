# Directus deploy

info will be placed here

## Generating the UNIQUE_KEY and UNIQUE_SECRET
A good and practical way to generate them would be to use:    
`openssl rand -hex 16` (output example: `ed84e94edee1a90bb82c1503fca9ce3e`)    
`openssl rand -base64 24` (output example: `NR0OLD1KXuZQu2aZyiGTzdmIEVy0IoLp`)

## Directus config reference:
https://docs.directus.io/self-hosted/config-options#general

# Commands
## backup database
    docker exec -t your-db-container mkdir -p /var/lib/postgresql/data/pg_dumps && pg_dumpall -c -U ${DB_USER} | gzip > /var/lib/postgresql/data/pg_dumps/$(date +"%Y-%m-%d_%H-%M-%S")_db-dump.sql.gz

## restore database (needs work)
    gunzip < your_dump.sql.gz | docker exec -i your-db-container psql -U ${DB_USER} -d ${PROJECT_NAME}-db