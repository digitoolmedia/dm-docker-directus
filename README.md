# Directus deploy

info will be placed here

## Generating the UNIQUE_KEY and UNIQUE_SECRET
A good and practical way to generate them would be to use:    
`openssl rand -hex 16` (output example: `ed84e94edee1a90bb82c1503fca9ce3e`)    
`openssl rand -base64 24` (output example: `NR0OLD1KXuZQu2aZyiGTzdmIEVy0IoLp`)

## Directus config reference:
https://docs.directus.io/self-hosted/config-options#general